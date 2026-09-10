# CDU-AD01: Iniciar Sesión en Panel de Administración

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe contar con unas credenciales válidas registradas previamente en la base de datos del sistema.
2. La cuenta del Administrador no debe encontrarse bajo un bloqueo activo de seguridad por intentos fallidos.

## Postcondiciones

1. El sistema genera una sesión web válida para el Administrador.
2. El sistema concede acceso y redirige al Administrador a la vista principal ("Dashboard") del Panel de Administración.

## Escenario Principal de Éxito (Flujo Básico)

1. El Administrador accede a la dirección web del Panel de Administración.
2. El Sistema muestra la pantalla de inicio de sesión solicitando las credenciales de administrador (Correo Electrónico y Contraseña).
3. El Administrador introduce sus credenciales (Correo Electrónico y Contraseña) y pulsa el botón de confirmación de acceso.
4. El Sistema verifica que las credenciales proporcionadas corresponden a un registro válido en la base de datos del sistema.
5. El Sistema inicializa la sesión del administrador y lo redirige a la vista "Dashboard", habilitando el Menú de Navegación con acceso a "Dashboard", "Nuevo Comunicado" e "Historial".

## Extensiones (Flujos Alternativos)

* **4a. Administrador no existente:**
    1. El Sistema muestra un mensaje de error genérico en la interfaz indicando que las credenciales introducidas no coinciden con ningún administrador.
    2. El Sistema permite al Administrador introducir nuevas credenciales. El caso de uso retorna al paso 3 del flujo básico.

* **4b. Credenciales incorrectas:**
    1. El Sistema muestra un mensaje de error genérico en la interfaz indicando que las credenciales introducidas no coinciden con ningún administrador.
    2. El Sistema incrementa en uno el contador de intentos fallidos consecutivos de la cuenta.
    3. El Sistema permite al Administrador corregir los datos. El caso de uso retorna al paso 3 del flujo básico.

* **4c. Superación del límite de protección contra fuerza bruta:**
    1. El Sistema detecta que el contador de intentos fallidos consecutivos ha alcanzado el límite máximo de 3 intentos.
    2. El Sistema bloquea temporalmente la cuenta del administrador en la base de datos.
    3. El Sistema muestra un aviso en la pantalla informando de que la cuenta ha sido suspendida por seguridad durante 24 horas.
    4. El Caso de Uso termina en fracaso.

## Requisitos Especiales

* **Seguridad de transmisión:** Toda la comunicación entre el navegador web del administrador y el servidor durante el proceso de autenticación debe realizarse mediante protocolos cifrados.
