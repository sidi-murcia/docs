# CDU-AF01: Iniciar Sesión en Aplicación Móvil

**Actor Principal:** Afiliado

## Precondiciones

1. El afiliado debe estar previamente registrado en el censo del sistema en estado "Activo".
2. No debe existir ninguna sesión activa de otro usuario en el terminal.
3. La cuenta del afiliado no debe encontrarse bajo un bloqueo activo de seguridad por intentos fallidos.

## Postcondiciones

1. El sistema genera una sesión válida para el afiliado.
2. El identificador físico del dispositivo móvil queda registrado y vinculado como el terminal activo del afiliado en la base de datos.
3. El sistema concede acceso al afiliado y lo redirige automáticamente a la interfaz principal de la aplicación.

## Escenario Principal de Éxito (Flujo Básico)

1. El Afiliado abre la aplicación móvil.
2. El Sistema detecta la ausencia de sesión activa y muestra la pantalla de inicio de sesión solicitando las credenciales (Número de Teléfono y el Correo Electrónico).
3. El Afiliado introduce sus credenciales (Número de Teléfono y Correo Electrónico) y pulsa el botón de confirmación de acceso.
4. El Sistema captura las credenciales introducidas junto con el identificador único de hardware del dispositivo móvil.
5. El Sistema valida que los datos coinciden exactamente con un registro del censo y que el estado del afiliado es "Activo".
6. El Sistema comprueba que el identificador del dispositivo coincide con el último terminal vinculado registrado para esa cuenta o que aún no hay un terminal vinculado.
7. El Sistema inicializa la sesión del usuario, concede el acceso y redirige al afiliado a la pantalla de canales de difusión.

## Extensiones (Flujos Alternativos)

* **5a. Afiliado no existente o en estado "Inactivo":**
    1. El Sistema muestra un mensaje de error genérico en la interfaz indicando que las credenciales introducidas no coinciden con ningún afiliado.
    2. El Sistema permite al Afiliado introducir nuevas credenciales. El caso de uso retorna al paso 3 del flujo básico.

* **5b. Credenciales incorrectas":**
    1. El Sistema muestra un mensaje de error genérico en la interfaz indicando que las credenciales introducidas no coinciden con ningún afiliado.
    2. El Sistema incrementa en uno el contador de intentos fallidos consecutivos de la cuenta.
    3. El Sistema permite al Afiliado corregir los datos. El caso de uso retorna al paso 3 del flujo básico.

* **5c. Superación del límite de protección contra fuerza bruta:**
    1. El Sistema detecta que el contador de intentos fallidos consecutivos ha alcanzado el límite máximo de 3 intentos.
    2. El Sistema bloquea temporalmente la cuenta del afiliado en la base de datos.
    3. El Sistema muestra un aviso en la pantalla informando de que la cuenta ha sido suspendida por seguridad durante 24 horas.
    4. El Caso de Uso termina en fracaso.

* **6a. Detección de cambio de dispositivo físico (Éxito):**
    1. El Sistema identifica que el código de hardware del terminal no coincide con el último registrado.
    2. El Sistema verifica que han transcurrido más de 48 horas desde la última vinculación de hardware válida.
    3. El Sistema actualiza el identificador en la cuenta y desvincula el dispositivo antiguo.
    4. El Caso de Uso continúa en el paso 7 del flujo básico.

* **6b. Violación del periodo de carencia por cambio de terminal (Fallo):**
    1. El Sistema identifica que el código de hardware del terminal no coincide y que **NO** han transcurrido las 48 horas obligatorias desde el último cambio.
    2. El Sistema deniega la vinculación del nuevo hardware y bloquea el acceso.
    3. El Sistema muestra una notificación de error indicando el tiempo restante necesario para poder migrar la cuenta.
    4. El Caso de Uso termina en fracaso.

## Requisitos Especiales

* **Seguridad de la información:** En ningún caso se almacenarán las credenciales del usuario en texto plano en el almacenamiento local del dispositivo.

* **Rendimiento:** El proceso completo de validación e intercambio de datos entre la app y el servidor de BD no debe superar un tiempo de respuesta (SLA) de 2 segundos.

* **Restricción de sesión única:** El sistema garantizará a nivel de base de datos la imposibilidad física de que una misma cuenta mantenga dos identificadores de hardware activos en paralelo.
