# REQUISITOS ESPECÍFICOS

Esta sección constituye el núcleo del documento, detallando exhaustivamente todas las interfaces, funciones y restricciones que el software debe satisfacer. Su propósito es establecer una base técnica inequívoca que sirva tanto de guía para el responsable del desarrollo como de criterio de aceptación para el cliente.

Para garantizar la calidad y rigor de estas especificaciones, la redacción y estructura de este apartado se han realizado siguiendo las directrices del estándar **IEEE Std 830-1998** (Recommended Practice for Software Requirements Specifications).

En cumplimiento con dicha normativa, se han aplicado los siguientes principios fundamentales:

* **Enfoque Funcional:** Cada requisito declara estrictamente “qué” debe realizar el sistema y no “cómo” debe ser implementado internamente.
* **Verificabilidad y Ausencia de Ambigüedad:** Se ha asegurado que cada requisito tenga una única interpretación posible y pueda ser verificado mediante un proceso finito y rentable para comprobar que el software cumple con lo especificado.
* **Necesidad:** Todos los requisitos listados son esenciales; su eliminación implicaría una deficiencia crítica en el producto final.

A continuación, se presentan los requisitos organizados por categorías lógicas, utilizando un identificador único para facilitar su trazabilidad.

## Requisitos de Interfaces Externas

### Interfaces de Usuario

* **[RI-USU-01]** La Aplicación Móvil debe mostrar la identidad visual (logotipo, paleta de colores y tipografía) definida en el Manual de Marca de SIDI en todas sus pantallas.
* **[RI-USU-02]** La Aplicación Móvil debe disponer de una pantalla de inicio de sesión que contenga los campos de entrada para las credenciales de afiliado (Número de Teléfono y Correo Electrónico) y el botón de acción “Entrar”.
* **[RI-USU-03]** La Aplicación Móvil debe mostrar un mensaje de error visible y descriptivo si la validación de credenciales falla, sin revelar cuál de los dos campos es incorrecto.
* **[RI-USU-04]** La Aplicación Móvil debe presentar al usuario autenticado una pantalla de selección de canal que muestre la lista completa de canales disponibles.
* **[RI-USU-05]** La Aplicación Móvil debe mostrar el listado de comunicados ordenados cronológicamente de forma inversa (el más reciente en la posición superior), permitiendo cargar comunicados anteriores al deslizar la pantalla.
* **[RI-USU-06]** La Aplicación Móvil debe mostrar, para cada comunicado del listado, su título, la fecha y hora de publicación, el nombre del canal de origen, el cuerpo del mensaje y, si la hubiera, una imagen adjunta.
* **[RI-USU-07]** La Aplicación Móvil debe incluir un botón visible que permita al afiliado cerrar la sesión activa.
* **[RI-USU-08]** El Panel de Administración debe mostrar la identidad visual (logotipo, paleta de colores y tipografía) definida en el Manual de Marca de SIDI en todas sus pantallas.
* **[RI-USU-09]** El Panel de Administración debe disponer de una pantalla de inicio de sesión que solicite las credenciales de administrador (Correo Electrónico y Contraseña).
* **[RI-USU-10]** El Panel de Administración debe disponer de un Menú de Navegación que permita alternar entre las vistas principales: “Dashboard”, “Nuevo Comunicado” e “Historial”.
* **[RI-USU-11]** El Panel de Administración debe incluir una opción accesible en el Menú de Navegación que permita importar un fichero de hoja de cálculo para la actualización de la base de datos de afiliados.
* **[RI-USU-12]** El Menú de Navegación debe incluir un botón visible para cerrar la sesión del administrador.
* **[RI-USU-13]** La Vista Dashboard debe mostrar un indicador numérico con el total de comunicados enviados en los últimos 7 días naturales.
* **[RI-USU-14]** La Vista Dashboard debe mostrar una gráfica de barras que represente la evolución diaria de envíos durante la semana actual, diferenciando por colores los envíos realizados a cada canal.
* **[RI-USU-15]** La Vista Nuevo Comunicado debe presentar un formulario de redacción con los siguientes campos: Título, Cuerpo del Mensaje y Adjuntar Imagen (máximo una imagen).
* **[RI-USU-16]** La Vista Nuevo Comunicado debe incluir un selector de canales que permita al administrador marcar uno, varios o todos los canales disponibles como destinatarios.
* **[RI-USU-17]** La Vista Nuevo Comunicado debe incluir un botón de “Enviar” que solicite una confirmación explícita al administrador antes de ejecutar el envío.
* **[RI-USU-18]** La Vista Nuevo Comunicado debe mostrar una notificación visual de éxito o error tras procesar el envío, limpiando el formulario automáticamente en caso de éxito.
* **[RI-USU-19]** La Vista Historial debe presentar una tabla de registros con todos los comunicados enviados, mostrando las siguientes columnas: Fecha y Hora, Título, Canal(es) de destino y Acción.
* **[RI-USU-20]** La Vista Historial debe mostrar acciones contextuales sobre cada comunicado para permitir editar su contenido o eliminarlo del sistema.
* **[RI-USU-21]** La Vista Historial debe incluir una barra de filtros que permita buscar comunicados por: rango de fechas (Desde/Hasta), texto en el título y selección de canal.

### Interfaces de Hardware

No aplica.

### Interfaces de Software

* **[RI-SW-01]** El sistema debe disponer de una base de datos propia e independiente que actúe como fuente única de verdad para la información de los afiliados y los comunicados.
* **[RI-SW-02]** El sistema debe disponer de un servicio de almacenamiento persistente para las imágenes adjuntas a los comunicados, accesible mediante URLs seguras.
* **[RI-SW-03]** El sistema debe integrar un servicio de notificaciones push compatible con los sistemas operativos Android e iOS.
* **[RI-SW-04]** La Aplicación Móvil debe persistir localmente los comunicados descargados, permitiendo consultarlos sin conexión a Internet.

### Interfaces de Comunicación

* **[RI-COM-01]** Todas las comunicaciones de red entre los componentes del sistema (Aplicación Móvil, Panel de Administración y Backend) deben realizarse mediante protocolos cifrados.

## Requisitos Funcionales

### Autenticación y Acceso

* **[RF-AUTH-01]** El sistema debe verificar que las credenciales proporcionadas por el afiliado (Número de Teléfono y Correo Electrónico) corresponden a un registro válido y activo en la base de datos del sistema antes de conceder acceso.
* **[RF-AUTH-02]** El sistema debe denegar el acceso a cualquier usuario cuyo estado en la base de datos no sea “activo”.
* **[RF-AUTH-03]** El sistema debe bloquear temporalmente el acceso de un afiliado durante 24 horas tras detectar 3 intentos fallidos consecutivos de inicio de sesión.
* **[RF-AUTH-04]** El sistema debe garantizar que una cuenta de afiliado esté vinculada en todo momento a un único dispositivo activo.
* **[RF-AUTH-05]** El sistema debe impedir la activación de un nuevo dispositivo para una cuenta de afiliado si no han transcurrido al menos 48 horas desde que dicho afiliado solicitó el cambio de dispositivo.
* **[RF-AUTH-06]** El sistema debe verificar que las credenciales proporcionadas por el administrador (Correo Electrónico y Contraseña) corresponden a un registro válido en la base de datos del sistema antes de conceder acceso al Panel de Administración.
* **[RF-AUTH-07]** El sistema debe bloquear temporalmente el acceso de un administrador durante 24 horas tras detectar 3 intentos fallidos consecutivos de inicio de sesión.

### Segmentación y Canales

* **[RF-SEG-01]** El sistema debe permitir a un afiliado autenticado estar suscrito a un máximo de dos canales de comunicación de forma concurrente.
* **[RF-SEG-02]** El sistema debe garantizar que el cambio de suscripción a un canal esté sujeto a una restricción temporal.
* **[RF-SEG-03]** El sistema debe permitir la creación de canales de comunicación, siendo los canales por defecto: “Funcionarios Maestros”, “Funcionarios Secundaria”, “Interinos Maestros”, “Interinos Secundaria”, “Funcionarios en Prácticas”, “Equipos Directivos Primaria” y “Equipos Directivos Secundaria”

### Difusión y Consumo (Broadcasting)

* **[RF-BRO-01]** El sistema debe enviar una notificación push a todos los dispositivos suscritos a los canales seleccionados inmediatamente después de que un administrador confirme el envío de un comunicado.
* **[RF-BRO-02]** El sistema debe garantizar que ningún afiliado tenga acceso a la lista de otros receptores ni a sus datos personales en ningún punto de la experiencia de uso.
* **[RF-BRO-03]** La Aplicación Móvil debe sincronizar automáticamente los comunicados con el servidor cada vez que esta sea abierta por el afiliado.
* **[RF-BRO-04]** La Aplicación Móvil debe sincronizar automáticamente los comunicados con el servidor tras la recepción de una notificación push.
* **[RF-BRO-05]** La Aplicación Móvil debe permitir la consulta de los comunicados previamente descargados sin necesidad de conexión a Internet.
* **[RF-BRO-06]** La Aplicación Móvil debe actualizar localmente cualquier comunicado cuyo contenido haya sido modificado en el servidor, reemplazando la versión anterior por la nueva.
* **[RF-BRO-07]** La Aplicación Móvil debe eliminar de la vista del afiliado cualquier comunicado que haya sido eliminado en el servidor.
* **[RF-BRO-08]** La Aplicación Móvil debe descargar y mostrar la imagen adjunta a un comunicado, si la hubiera.

### Administración

* **[RF-ADM-01]** El sistema debe procesar un fichero de hoja de cálculo proporcionado desde el Panel de Administración para dar de alta, actualizar o dar de baja afiliados en la base de datos del sistema.
* **[RF-ADM-02]** El sistema debe revocar automáticamente el acceso a la Aplicación Móvil a aquellos afiliados cuyo estado pase a “baja” tras la importación de una hoja de cálculo.
* **[RF-ADM-03]** El sistema debe proporcionar el número total de comunicados publicados en los últimos 7 días naturales.
* **[RF-ADM-04]** El sistema debe proporcionar datos agregados del volumen de comunicados enviados desglosados por día y por canal de destino.
* **[RF-ADM-05]** El sistema debe validar que los campos obligatorios de un comunicado (Título y Cuerpo del Mensaje) no estén vacíos y que se haya seleccionado al menos un canal de destino o grupo de afiliados antes de permitir su envío.
* **[RF-ADM-06]** El sistema debe aceptar y almacenar una imagen adjunta por comunicado, siempre que su tamaño no exceda los 5 MB.
* **[RF-ADM-07]** El sistema debe permitir la búsqueda y filtrado del historial de comunicados por: coincidencia de texto en el título, canal de destino y rango de fechas de publicación.
* **[RF-ADM-08]** El sistema debe permitir la modificación del contenido (Título, Cuerpo del Mensaje e Imagen) de un comunicado previamente enviado.
* **[RF-ADM-09]** El sistema debe enviar una nueva notificación push a los destinatarios originales del comunicado, ya sean canales o grupos personalizados de afiliados, tras su modificación.
* **[RF-ADM-10]** El sistema debe permitir al administrador seleccionar afiliados individuales como destinatarios de un comunicado, mediante un buscador por nombre, NIF o teléfono, componiendo así un grupo de envío personalizado.
* **[RF-ADM-11]** El sistema debe permitir la eliminación de un comunicado, haciendo que deje de estar disponible en los dispositivos de los afiliados.
* **[RF-ADM-12]** El sistema debe identificar en el historial de comunicados si el envío fue dirigido a un canal o a un grupo personalizado de afiliados, mostrando en este último caso el número de destinatarios.
* **[RF-ADM-13]** El Panel de Administración debe permitir al administrador crear nuevos canales, modificar el nombre de los canales existentes, así como desactivarlos y reactivarlos.

## Requisitos de Rendimiento

* **[RNF-REN-01]** El Backend debe procesar las peticiones de lectura de comunicados con un tiempo de respuesta promedio inferior a 800 milisegundos bajo condiciones normales de carga.
* **[RNF-REN-02]** La Aplicación Móvil debe presentar los comunicados almacenados localmente en un tiempo no superior a 5 segundos tras la apertura de la aplicación.
* **[RNF-REN-03]** La Aplicación Móvil debe limitar el almacenamiento local a los últimos 50 comunicados para controlar el uso de espacio en el dispositivo del afiliado.

## Restricciones de Diseño

No aplica.

## Atributos del sistema software

* **[RNF-ATR-01]** El código fuente del proyecto debe seguir los estándares de estilo y formato idiomáticos de cada tecnología empleada para asegurar la legibilidad y facilitar futuras auditorías o traspasos de conocimiento.
* **[RNF-ATR-02]** La compilación de la Aplicación Móvil debe incorporar mecanismos de protección contra ingeniería inversa para dificultar la extracción de lógica de negocio o credenciales sensibles.
* **[RNF-ATR-03]** El sistema debe disponer de un mecanismo de respaldo automático de datos que garantice la recuperación del historial de comunicados y de la información de afiliados ante un fallo.

## Dependencias del Sistema

* **[DP-01]** El sistema requiere que la Organización mantenga activas y vigentes las cuentas de desarrollador en Apple App Store y Google Play Store.
* **[DP-02]** La redacción y validación jurídica de los “Términos y Condiciones de Uso” y la “Política de Privacidad” son responsabilidad exclusiva de la Organización. El equipo de desarrollo se limita a su inserción técnica en las interfaces.
* **[DP-03]** La publicación final de las aplicaciones está supeditada a los procesos de revisión y aprobación de Apple App Store y Google Play Store. Aunque el desarrollo cumplirá con las guías técnicas oficiales, los tiempos de revisión y las solicitudes de cambio por parte de estas plataformas son externos al control del equipo de desarrollo.
* **[DP-04]** El despliegue del sistema requiere que el Cliente financie un entorno de alojamiento que cumpla con los requisitos técnicos definidos en el diseño del sistema, bajo el criterio del consultor.
* **[DP-05]** El Cliente asumirá los costes derivados de servicios de terceros necesarios para la operación del sistema (alojamiento, almacenamiento, cuentas de desarrollador, etcétera).
* **[DP-06]** El Manual de Marca de SIDI (logotipo, paleta de colores, tipografía) debe ser proporcionado por la Organización antes del inicio de la fase de diseño de interfaces.
