# CU-AD03: Importar Censo de Afiliados

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe tener una sesión web activa y válida en el Panel de Administración.
2. El Administrador debe disponer de un fichero de hoja de cálculo (ej. formato `.xlsx` o `.csv`) estructurado con los datos del censo actualizados.

## Postcondiciones

1. La base de datos del sistema queda sincronizada con la información del fichero, reflejando las nuevas altas, modificaciones y bajas.
2. Se revoca automáticamente el acceso a la Aplicación Móvil a aquellos afiliados cuyo estado haya pasado a "baja".

## Escenario Principal de Éxito (Flujo Básico)

1. El Administrador accede a la opción de importar hoja de cálculo desde el Menú de Navegación del Panel de Administración.
2. El Sistema presenta una interfaz visual para la carga de ficheros.
3. El Administrador selecciona el fichero del censo de afiliados y confirma la subida al servidor.
4. El Sistema valida que el formato del fichero es correcto y que contiene las columnas de datos obligatorias (NIF, Correo Electrónico, Número de Teléfono, Estado).
5. El Sistema procesa de forma transaccional la sincronización del censo unificando las bajas explícitas e implícitas:
    * Si el identificador (NIF) está en el fichero, el Sistema inserta un nuevo registro o actualiza el existente.
    * Si un identificador (NIF) ya existía en la base de datos del sistema pero **no está presente** en el fichero importado, el Sistema actualiza automáticamente su estado interno a "Baja".
6. Durante el procesamiento, el Sistema invalida de forma automática la sesión y los tokens de acceso de cualquier afiliado que haya pasado a estado "Baja".
7. El Sistema finaliza el proceso y muestra en pantalla un resumen de la operación (número de altas nuevas, registros actualizados y bajas procesadas).

## Extensiones (Flujos Alternativos)

* **4a. Formato de fichero inválido o ilegible:**
    1. El Sistema detecta que el fichero subido no es una hoja de cálculo válida o se encuentra dañado.
    2. El Sistema detiene el proceso y muestra un mensaje de error indicando que el fichero no puede ser leído.
    3. El Caso de Uso retorna al paso 2.

* **4b. Ausencia de columnas obligatorias:**
    1. El Sistema detecta que el fichero carece de campos críticos necesarios para la lógica de negocio.
    2. El Sistema aborta la importación de forma segura, sin alterar la base de datos.
    3. El Sistema muestra un aviso detallando qué columnas exactas faltan para que el Administrador corrija el documento de origen.
    4. El Caso de Uso retorna al paso 2.

## Requisitos Especiales

* **Independencia Operativa:** Este proceso materializa el desacople del sistema de difusión respecto a bases de datos de terceros, operando de forma autónoma con los datos estrictamente necesarios.
* **Transaccionalidad (Seguridad de base de datos):** La importación debe tratarse como una única transacción de base de datos (o procesarse en lotes seguros) para evitar que, en caso de caída del servidor a mitad del proceso, el censo quede en un estado inconsistente.
* **Preservación de Identificadores:** El paso a estado "Baja" de un afiliado no debe borrar físicamente su registro de la base de datos (Soft Delete).
