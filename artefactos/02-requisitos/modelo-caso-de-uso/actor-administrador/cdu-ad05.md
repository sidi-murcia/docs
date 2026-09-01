# CU-AD02: Emitir Nuevo Comunicado

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe tener una sesión web activa y válida en el Panel de Administración.

## Postcondiciones

1. El comunicado (junto con su imagen adjunta, si existe) queda almacenado de forma persistente en la base de datos y en el servicio de almacenamiento de objetos del sistema.
2. El sistema envía una notificación push de forma asíncrona a los dispositivos móviles de todos los afiliados suscritos a los canales seleccionados o incluidos en el grupo personalizado.

## Escenario Principal de Éxito (Flujo Básico)

1. El Administrador accede a la vista "Nuevo Comunicado" desde el Menú de Navegación del Panel de Administración.
2. El Sistema presenta el formulario de redacción de comunicados.
3. El Administrador introduce el texto en los campos obligatorios correspondientes al Título y al Cuerpo del Mensaje.
4. El Administrador selecciona opcionalmente un archivo de imagen desde su equipo local para adjuntarlo al comunicado.
5. El Administrador define los destinatarios seleccionando uno, varios o todos los canales disponibles en el selector.
6. El Administrador pulsa el botón "Enviar".
7. El Sistema muestra un cuadro de diálogo solicitando una confirmación explícita para ejecutar el envío masivo.
8. El Administrador confirma la acción.
9. El Sistema valida que los campos obligatorios contienen información, que existe al menos un destinatario seleccionado y que el tamaño de la imagen adjunta cumple con los límites establecidos.
10. El Sistema almacena la información de forma definitiva e invoca al servicio externo de notificaciones push para su distribución.
11. El Sistema muestra una notificación visual de éxito en la interfaz y limpia el formulario automáticamente para permitir un nuevo envío.

## Extensiones (Flujos Alternativos)

* **5a. El Administrador define un grupo personalizado en lugar de canales:**

 1. En el paso de selección de destinatarios, el Administrador utiliza el buscador integrado para localizar afiliados específicos por nombre, NIF o teléfono.
 2. El Administrador selecciona a los afiliados deseados componiendo un grupo de envío personalizado.
 3. El Caso de Uso retorna al paso 6 del flujo básico.

* **8a. El Administrador cancela la confirmación de envío:**

 1. En el cuadro de confirmación, el Administrador pulsa la opción de cancelar.
 2. El Sistema oculta el cuadro de diálogo y mantiene intactos todos los datos introducidos en el formulario.
 3. El Caso de Uso retorna al paso 5 del flujo básico.

* **9a. Validación fallida por campos vacíos:**

 1. El Sistema detecta que el Título, el Cuerpo del Mensaje o los destinatarios (canales o grupo) se encuentran vacíos.
 2. El Sistema detiene el proceso, mantiene los datos introducidos y muestra un mensaje de error advirtiendo al Administrador sobre los campos requeridos faltantes.
 3. El Caso de Uso retorna al paso 3.

* **9b. Validación fallida por exceso de tamaño en la imagen:**

 1. El Sistema detecta que el archivo de imagen seleccionado excede el tamaño máximo permitido de 5 MB.
 2. El Sistema detiene el proceso y muestra un mensaje de error indicando que el archivo es demasiado grande.
 3. El Caso de Uso retorna al paso 4.

## Requisitos Especiales

* **Asincronía en el envío:** El proceso de comunicación con el servicio push externo (FCM) debe realizarse de forma asíncrona (en segundo plano) para evitar bloquear la interfaz del Panel de Administración durante los envíos masivos.
