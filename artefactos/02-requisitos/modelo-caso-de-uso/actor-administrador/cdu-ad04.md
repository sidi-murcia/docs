# CU-AD04: Gestionar canales

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe tener una sesión web activa y válida en el Panel de Administración.

## Postcondiciones

1. El sistema actualiza el catálogo de canales de comunicación disponibles, reflejando las creaciones, modificaciones, activaciones o desactivaciones.

## Escenario Principal de Éxito (Flujo Básico: Consulta)

1. El Administrador accede a la sección de gestión de canales desde el Menú de Navegación del panel web.
2. El Sistema recupera y muestra el listado completo de canales de comunicación existentes (tanto activos como inactivos).
3. El Administrador revisa la lista de canales.
4. El Caso de Uso termina.

## Extensiones (Flujos Alternativos)

* **3a. El Administrador desea crear un nuevo canal:**
    1. El Administrador selecciona la acción de crear un nuevo canal.
    2. El Sistema muestra un formulario solicitando el nombre del nuevo canal.
    3. El Administrador introduce el nombre y confirma.
    4. El Sistema valida que el nombre no esté duplicado ni vacío.
    5. El Sistema genera un identificador aleatorio único para el topic de FCM asociado (`canal_<uuid>`) y registra el nuevo canal en la base de datos como activo (`activo = true`), habilitándolo para las suscripciones de los afiliados y actualizando la lista en pantalla.
    6. El Caso de Uso retorna al paso 3.

* **3b. El Administrador desea modificar el nombre de un canal existente (renombrado):**
    1. El Administrador selecciona la acción de editar sobre un canal de la lista.
    2. El Sistema habilita la modificación del nombre del canal.
    3. El Administrador modifica el texto y confirma los cambios.
    4. El Sistema valida el nuevo nombre (que no esté vacío ni duplicado) y actualiza el registro en la base de datos manteniendo inalterado el topic de FCM.
    5. El Caso de Uso retorna al paso 3.

* **3c. El Administrador desea desactivar un canal:**
    1. El Administrador selecciona la acción de desactivar sobre un canal activo de la lista.
    2. El Sistema muestra un aviso advirtiendo que los afiliados suscritos perderán la suscripción a dicho canal.
    3. El Administrador confirma la desactivación de forma explícita.
    4. El Sistema desactiva el canal en la base de datos (`activo = false`), desvinculando automáticamente a todos los afiliados que estuvieran suscritos a él (desactivando sus suscripciones en cascada).
    5. El Sistema desvincula a los usuarios del topic de FCM en segundo plano.
    6. El Sistema actualiza la lista mostrada en pantalla.
    7. El Caso de Uso retorna al paso 3.

* **3d. El Administrador desea activar un canal previamente desactivado:**
    1. El Administrador selecciona la acción de activar sobre un canal inactivo de la lista.
    2. El Sistema valida que el canal esté efectivamente desactivado.
    3. El Sistema activa el canal en la base de datos (`activo = true`), dejándolo nuevamente disponible para que los afiliados puedan suscribirse y recibir comunicados (las suscripciones previas permanecen inactivas).
    4. El Sistema actualiza la lista mostrada en pantalla.
    5. El Caso de Uso retorna al paso 3.

* **3e. Fallo de validación:**
    1. Si el Administrador introduce un nombre vacío o que coincide con el de otro canal existente (ramas 3a y 3b), el Sistema detiene la operación, muestra un mensaje de error y permite corregir el nombre.
    2. Si el Administrador intenta activar un canal que ya está activo o desactivar un canal que ya está inactivo (ramas 3c y 3d), el Sistema rechaza la operación e informa de la inconsistencia de estado.

## Requisitos Especiales

**Implementación de Notificaciones (FCM)**: La gestión de canales tiene como fuente de verdad la base de datos relacional del sistema. Para evitar colisiones y permitir el renombrado libre de los canales sin efectos colaterales en la infraestructura de mensajería, el nombre del topic FCM asignado a cada canal es generado de forma aleatoria en el momento de su creación. Al desactivar un canal, este y sus suscripciones se desactivan en la base de datos, y el backend desvincula a los usuarios del topic de FCM de manera asíncrona (en segundo plano) mediante el Firebase Admin SDK (agrupando las peticiones en lotes de máximo 1.000 dispositivos) para no demorar ni bloquear la respuesta HTTP. En caso de reactivación del canal, se conserva su topic FCM original.
