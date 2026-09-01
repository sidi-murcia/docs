# CU-AD04: Gestionar canales

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe tener una sesión web activa y válida en el Panel de Administración.

## Postcondiciones

1. El sistema actualiza el catálogo de canales de comunicación disponibles, reflejando las creaciones, modificaciones o eliminaciones.

## Escenario Principal de Éxito (Flujo Básico: Consulta)

1. El Administrador accede a la sección de gestión de canales desde el Menú de Navegación del panel web.
2. El Sistema recupera y muestra el listado completo de canales de comunicación existentes.
3. El Administrador revisa la lista de canales.
4. El Caso de Uso termina.

## Extensiones (Flujos Alternativos)

* **3a. El Administrador desea crear un nuevo canal:**
    1. El Administrador selecciona la acción de crear un nuevo canal.
    2. El Sistema muestra un formulario solicitando el nombre del nuevo canal.
    3. El Administrador introduce el nombre y confirma.
    4. El Sistema valida que el nombre no esté duplicado ni vacío.
    5. El Sistema registra el nuevo canal en la base de datos, lo habilita para las suscripciones de los afiliados y actualiza la lista en pantalla.
    6. El Caso de Uso retorna al paso 3.

* **3b. El Administrador desea modificar el nombre de un canal existente:**
    1. El Administrador selecciona la acción de editar sobre un canal de la lista.
    2. El Sistema habilita la modificación del nombre del canal.
    3. El Administrador modifica el texto y confirma los cambios.
    4. El Sistema valida el nuevo nombre y actualiza el registro en la base de datos.
    5. El Caso de Uso retorna al paso 3.

* **3c. El Administrador desea eliminar un canal:**
    1. El Administrador selecciona la acción de eliminar sobre un canal de la lista.
    2. El Sistema muestra un aviso advirtiendo que los afiliados perderán la suscripción a dicho canal y solicita confirmación.
    3. El Administrador confirma la eliminación de forma explícita.
    4. El Sistema elimina el canal de la base de datos, desvinculando automáticamente a todos los afiliados que estuvieran suscritos a él (borrado en cascada de suscripciones).
    5. El Sistema actualiza la lista mostrada en pantalla.
    6. El Caso de Uso retorna al paso 3.

* **3d. Fallo de validación (aplica a las ramas 3a y 3b):**
    1. El Sistema detecta que el nombre introducido para el canal se encuentra vacío o coincide con el de un canal ya existente.
    2. El Sistema detiene la operación, muestra un mensaje de error y permite al Administrador corregir el nombre.

## Requisitos Especiales

* **Implementación de Notificaciones (FCM):** La creación, modificación o borrado de canales opera exclusivamente sobre la base de datos relacional del sistema. No se requiere sincronización en tiempo real con la API del servicio de notificaciones push (FCM), ya que la infraestructura de topics de Firebase es implícita y se gestiona dinámicamente durante la suscripción del cliente o la emisión de comunicados.