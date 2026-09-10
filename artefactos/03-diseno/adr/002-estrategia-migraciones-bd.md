# ADR 002: Estrategia de Migraciones de Base de Datos y Despliegues

## Estado

Aceptado

## Contexto

El sistema SIDI requiere un mecanismo fiable y versionado para aplicar cambios estructurales (DDL) y de datos (DML) a su base de datos PostgreSQL en todos los entornos. Se ha debatido la necesidad de aplicar el patrón **Expand and Contract** (Zero-Downtime Deployments) frente a estrategias más sencillas que asumen una breve ventana de inactividad durante el despliegue.

## Decisión

Se ha decidido **descartar el patrón Expand and Contract** debido a la sobreingeniería y fricción operativa (múltiples despliegues para un solo cambio lógico) que introduciría en la escala actual del equipo y del sistema.

En su lugar, el proyecto adoptará una **Estrategia de Ventana de Mantenimiento (Recreate / Stop-the-World)**, orquestada de la siguiente manera:

1. **Herramienta:** Uso estricto de `golang-migrate/migrate` con ficheros SQL puros versionados por **Timestamp** (Unix Time).
2. **Ejecución Desacoplada:** Las migraciones nunca se ejecutarán embebidas en el `main.go` de la aplicación para evitar problemas con las Liveness Probes y bloqueos de red.
3. **Flujo de Despliegue:**
   - Detener el contenedor de la API (interrumpiendo el tráfico y asumiendo downtime).
   - Ejecutar la migración estructural mediante un Init Container o Script dedicado.
   - Desplegar la nueva versión de la API compatible con el nuevo esquema.

## Consecuencias

- **Positivas:**
  - Drástica simplificación en el código de aplicación, que no tiene que lidiar con compatibilidad hacia atrás o lógica de "doble escritura".
  - Las migraciones complejas (como renombrar o borrar columnas) se pueden realizar en un solo paso (`ALTER TABLE ...`).
  - Reducción del tiempo de ciclo de vida de las tareas.

- **Negativas:**
  - La aplicación experimentará una caída del servicio (`502 Bad Gateway` o `Connection Refused`) durante 10-30 segundos en cada actualización que requiera tocar la BD. Esta ventana se considera aceptable por el negocio.
