# AGENTS.md

## 1. Propósito de este Repositorio

- Este repositorio (`docs/`) es la **Única Fuente de la Verdad (SSOT)** para el Sistema de Difusión Sindical (SIDI).
- Aquí NO hay código fuente. Este repositorio contiene los artefactos del Proceso Unificado (UP) que dictan el comportamiento, los requisitos y el diseño estricto que deben seguir todos los repositorios de código.
- **Regla de oro para agentes de IA:** NUNCA inventes requisitos, flujos, estructuras de datos, nombres de tablas ni contratos de red. TODO desarrollo debe basarse en la lectura previa de los artefactos listados a continuación.

## 2. Mapa de Artefactos (Directorio de Contratos)

Antes de generar código en cualquier otro repositorio, debes consultar los siguientes artefactos según la tarea encomendada:

### A. Dominio y Vocabulario

- **Ruta:** `artefactos/01-modelado-del-negocio/glosario-dominio.md`
- **Uso:** Utiliza exclusivamente los nombres y cardinalidades de entidades aquí definidos (`Administrador`, `Afiliado`, `Comunicado`, `Canal`, `SuscritoA`, `DispositivoMovil`, `Imagen`). Prohibido usar sinónimos en el código.

### B. Funcionalidad y Reglas de Negocio

- **Rutas:**
  - `artefactos/02-requisitos/modelo-casos-de-uso/actor-administrador/cdu-ad*.md`
  - `artefactos/02-requisitos/modelo-casos-de-uso/actor-afiliado/cdu-af*.md`
- **Uso:** Estos ficheros contienen los flujos principales, alternativos y pre/postcondiciones. La lógica de tus servicios/casos de uso en el código debe ser un reflejo exacto de estos pasos.
- **Restricciones Transversales:** Aplica siempre las reglas definidas en `artefactos/02-requisitos/especificacion-complementaria/requisitos-especificos.md`.

### C. Diseño Técnico y Contratos

- **Modelo de Datos Relacional:** `artefactos/03-diseno/modelo-datos.md`
  - El esquema de base de datos y los modelos ORM deben respetar estas relaciones exactamente.
- **Contrato REST API (Tolerancia Cero a Desviaciones):** `artefactos/03-diseno/openapi.yaml`
  - Todo endpoint, controlador, DTO (Data Transfer Object) y llamada HTTP desde frontend/móvil DEBE coincidir con este esquema OpenAPI. Está estrictamente prohibido añadir, modificar o ignorar campos de este contrato en la implementación.

## 3. Protocolo de Resolución de Ambigüedades

- Si durante la implementación en un repositorio de código encuentras una contradicción entre un Caso de Uso y el `openapi.yaml`, **detén la ejecución**.
- Notifica al ingeniero (usuario) sobre la discrepancia referenciando ambos artefactos para que el conflicto se resuelva en el diseño antes de escribir el código.

## 4. Estrategia de Migraciones de Base de Datos

El **backend** es el único "dueño" del esquema de la base de datos relacional.

- **Herramienta:** El proyecto utiliza `golang-migrate/migrate` (con scripts en SQL puro) en lugar de herramientas basadas en JVM (como Liquibase).
- **Ubicación:** Las migraciones viven físicamente en el repositorio del backend (ej. `backend/db/migrations/`).
- **Flujo:** Ningún otro componente (ni la app, ni la web) debe acceder directamente a la base de datos ni intentar migrarla. Las migraciones se ejecutan durante el despliegue del backend.
