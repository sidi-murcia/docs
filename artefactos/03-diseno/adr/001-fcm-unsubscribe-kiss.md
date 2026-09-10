# ADR: Estrategia de desuscripción de FCM al desactivar canales

## Contexto

En el sistema de comunicación, al desactivar lógicamente un canal, necesitamos desuscribir a los usuarios del Topic correspondiente en FCM. Surgió la duda de si rotar el ID del Topic para evitar "usuarios fantasma" en caso de que la red falle a mitad de la desuscripción, o si programar reintentos (retry backoff).

## Decisión

Se decide mantener el mismo Topic ID de FCM asociado al canal y ejecutar una desuscripción masiva simple (por lotes) delegada a una goroutine, sin lógicas de reintento.

## Consecuencias

* Positivo: Mantenemos la capa de infraestructura y casos de uso simples y limpios.
* Negativo (Riesgo Asumido): Si la API de Firebase falla por un problema de red en ese instante concreto, podrían quedar usuarios suscritos a un canal inactivo. Dada la volumetría esperada (máx 10.000 usuarios) y la rareza de la desactivación de canales, el riesgo es aceptable.
