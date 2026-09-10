# CU-AD07: Cerrar Sesión en Panel de Administración

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe tener una sesión web activa y válida en el Panel de Administración.

## Postcondiciones

1. La sesión actual queda invalidada y los tokens de acceso son destruidos.
2. El sistema redirige al Administrador a la pantalla de inicio de sesión.

## Escenario Principal de Éxito (Flujo Básico)

1. El Administrador selecciona la opción "Cerrar sesión" disponible en el Menú de Navegación del panel web.
2. El Sistema presenta un cuadro de diálogo solicitando confirmación para finalizar la sesión.
3. El Administrador confirma la acción.
4. El Sistema elimina de forma segura los tokens de acceso y cualquier dato sensible almacenado en la memoria local del navegador web (Local Storage / Session Storage / Cookies). No se realiza ninguna llamada al backend dado que la autenticación es sin estado (stateless).
5. El Sistema actualiza la interfaz, bloquea el acceso a las vistas de administración y redirige al Administrador a la pantalla de autenticación (CU-AD01).

## Extensiones (Flujos Alternativos)

* **3a. El Administrador cancela el cierre de sesión:**

 1. En el cuadro de confirmación, el Administrador selecciona la opción de cancelar.
 2. El Sistema oculta el cuadro de diálogo y mantiene la sesión web activa sin interrupciones.
 3. El Caso de Uso termina.

## Requisitos Especiales

* **Borrado de Caché Visual (Seguridad):** Al tratarse de un panel de administración que expone datos privados (como el censo de afiliados), el proceso de cierre de sesión debe garantizar que el navegador no conserve vistas cacheadas. Esto previene que una persona no autorizada pueda ver el historial o el panel pulsando el botón "Atrás" del navegador tras el cierre de sesión.
