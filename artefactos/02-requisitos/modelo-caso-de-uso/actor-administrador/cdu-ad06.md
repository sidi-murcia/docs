# CU-AD06: Gestionar Historial de Comunicados

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe tener una sesión web activa y válida en el Panel de Administración.

## Postcondiciones

1. El historial de comunicados del sistema queda actualizado tras cualquier modificación o eliminación.
2. Tras la edición de un comunicado, el sistema envía una nueva notificación push a los destinatarios originales.
3. Tras la eliminación de un comunicado, este deja de estar disponible en las aplicaciones móviles de los afiliados.

## Escenario Principal de Éxito (Flujo Básico: Consultar)

1. El Administrador accede a la vista "Historial" desde el Menú de Navegación del Panel de Administración.
2. El Sistema recupera y presenta una tabla de registros con todos los comunicados enviados.
3. El Sistema muestra para cada registro la Fecha y Hora, el Título, los destinatarios (indicando el canal o, si es un grupo personalizado, el número de destinatarios) y las acciones contextuales disponibles.
4. El Administrador revisa el listado de comunicados.
5. El Caso de Uso termina.

## Extensiones (Flujos Alternativos)

* **4a. El Administrador aplica filtros de búsqueda:**

 1. En la barra de filtros, el Administrador introduce un rango de fechas (Desde/Hasta), texto coincidente para el título o selecciona un canal específico.
 2. El Sistema procesa los criterios, filtra los resultados de la base de datos y actualiza la tabla mostrada en pantalla.
 3. El Caso de Uso retorna al paso 4.

* **4b. El Administrador edita un comunicado existente:**

 1. El Administrador selecciona la acción contextual de editar sobre un comunicado específico del historial.
 2. El Sistema presenta el formulario de edición cargando los datos actuales del comunicado (Título, Cuerpo del Mensaje e Imagen).
 3. El Administrador modifica el contenido deseado y confirma los cambios.
 4. El Sistema valida que los campos obligatorios no estén vacíos y que la nueva imagen (si la hubiera) no supere los 5 MB de tamaño.
 5. El Sistema actualiza el registro en la base de datos y en el almacenamiento de objetos.
 6. El Sistema envía una nueva notificación push dirigiéndose exclusivamente a los destinatarios originales del comunicado alertando de la modificación.
 7. El Caso de Uso retorna al paso 2.

* **4c. El Administrador elimina un comunicado:**

 1. El Administrador selecciona la acción contextual de eliminar sobre un comunicado específico del historial.
 2. El Sistema presenta un cuadro de diálogo solicitando confirmación explícita.
 3. El Administrador confirma la eliminación.
 4. El Sistema elimina el comunicado del servidor y borra la imagen adjunta asociada.
 5. El Sistema instruye a la Aplicación Móvil (mediante sincronización o borrado en cascada) para que el comunicado desaparezca de la vista de los afiliados.
 6. El Caso de Uso retorna al paso 2.

* **4d. Validación fallida durante la edición (aplica a la rama 4b):**

 1. El Sistema detecta que el Título o el Cuerpo del Mensaje han quedado vacíos, o que la nueva imagen adjunta excede los 5 MB.
 2. El Sistema detiene el proceso de guardado, mantiene los datos introducidos en el formulario y muestra un mensaje de error específico para que el Administrador lo corrija.
 3. El Caso de Uso retorna al paso 3 de la extensión 4b.

## Requisitos Especiales

* **Inmutabilidad de Destinatarios:** Durante la edición de un comunicado (extensión 4b), el Sistema no permite modificar los canales de destino ni el grupo personalizado original, restringiendo la edición únicamente al contenido (Título, Cuerpo e Imagen).
