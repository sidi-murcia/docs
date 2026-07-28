# CDU-AF02: Gestionar Suscripciones a Canales

**Actor Principal:** Afiliado

## Precondiciones

1. El Afiliado debe tener una sesión activa y válida en la aplicación móvil.
2. El dispositivo móvil debe contar con conectividad a la red.

## Postcondiciones

1. Las preferencias de suscripción del Afiliado se actualizan en la base de datos del sistema.
2. El Afiliado queda habilitado (o inhabilitado) para recibir notificaciones de los canales modificados.

## Escenario Principal de Éxito (Flujo Básico)

1. El Afiliado accede a la sección de gestión de canales en la aplicación móvil.
2. El Sistema recupera la lista de canales activos disponibles en el sindicato y el estado actual de suscripción del Afiliado para cada uno de ellos.
3. El Sistema presenta los canales, diferenciando visualmente aquellos a los que el Afiliado ya está suscrito.
4. El Afiliado selecciona la opción de "Suscribirse" en un canal específico.
5. El Sistema registra la nueva vinculación entre el Afiliado y el canal seleccionado.
6. El Sistema actualiza la interfaz mostrando el canal como "Suscrito" y confirma la acción al usuario.

## Extensiones (Flujos Alternativos)

* **3a. No existen canales activos en el sistema:**
    1. El Sistema informa al Afiliado mediante un mensaje en la interfaz de que actualmente no hay canales de difusión disponibles.
    2. El Caso de Uso termina.

* **4a. El Afiliado selecciona "Desuscribirse" de un canal:**
    1. El Afiliado selecciona la opción de anular su suscripción a un canal en el que ya estaba inscrito.
    2. El Sistema elimina la vinculación entre el Afiliado y el canal en sus registros.
    3. El Sistema actualiza la interfaz mostrando el canal como "No suscrito".
    4. El Caso de Uso retorna al paso 6 del flujo básico.

* **5a. Canal eliminado o inactivo durante la operación:**
    1. Al intentar registrar la suscripción, el Sistema detecta que el canal ha sido eliminado o desactivado por la Junta Directiva (milisegundos antes).
    2. El Sistema deniega la suscripción y muestra un aviso indicando que el canal ya no se encuentra disponible.
    3. El Sistema refresca la lista de canales (retorna al paso 2).

## Requisitos Especiales

* **Patrón de Notificación (Arquitectura):** La suscripción implica la recepción de alertas push en tiempo real. Por motivos de seguridad y eficiencia, la infraestructura de notificaciones de terceros solo se usará para enviar el aviso de "Nuevo comunicado"; la descarga del contenido real del comunicado se realizará siempre bajo demanda mediante peticiones directas y autenticadas desde la app hacia el servidor central.
