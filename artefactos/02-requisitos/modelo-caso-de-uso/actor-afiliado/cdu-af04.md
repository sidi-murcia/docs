# CDU-AF04: Cerrar Sesión en Aplicación Móvil

**Actor Principal:** Afiliado

## Precondiciones

1. El Afiliado debe tener una sesión activa y válida en la aplicación móvil.

## Postcondiciones

1. Las credenciales y tokens de acceso son eliminados del almacenamiento del dispositivo.
2. La aplicación cancela la suscripción local a las notificaciones Push (FCM).
3. Se eliminan los datos confidenciales cacheados en el almacenamiento local del dispositivo.
4. El sistema redirige al Afiliado a la pantalla de inicio de sesión.

## Escenario Principal de Éxito (Flujo Básico)

1. El Afiliado selecciona la opción "Cerrar sesión" en el menú de la aplicación.
2. El Sistema presenta un cuadro de diálogo solicitando confirmación.
3. El Afiliado confirma que desea cerrar la sesión.
4. El Sistema invoca al servicio de notificaciones (Firebase) para invalidar el token de recepción en el dispositivo.
5. El Sistema elimina de forma segura el token de autenticación (JWT) y la caché local de comunicados descargados.
6. El Sistema actualiza la interfaz y redirige al Afiliado a la pantalla de autenticación (CU-A01).

## Extensiones (Flujos Alternativos)

* **3a. El Afiliado cancela el cierre de sesión:**
    1. El Afiliado pulsa "Cancelar" en el cuadro de confirmación.
    2. El Sistema oculta el cuadro de diálogo y mantiene la sesión intacta.
    3. El Caso de Uso termina.

## Requisitos Especiales

* **Persistencia del Identificador de Hardware:** Por exigencias de la regla de seguridad de "carencia de 48 horas", el cierre de sesión local **NO** emite ninguna orden al servidor para borrar el identificador de hardware de la cuenta. 
* **Borrado Criptográfico (Privacidad):** La destrucción de la caché de comunicados (paso 5) debe ser absoluta para evitar que otro usuario en el mismo terminal acceda a información residual.