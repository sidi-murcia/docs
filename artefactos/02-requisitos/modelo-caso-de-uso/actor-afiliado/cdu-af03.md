# CDU-AF03: Consultar Comunicados

**Actor Principal:** Afiliado

## Precondiciones

1. El Afiliado debe tener una sesión activa y válida en la aplicación móvil.
2. El dispositivo móvil debe contar con conectividad a la red (para sincronizar nuevos comunicados).

## Postcondiciones

1. El Afiliado visualiza el listado actualizado de sus comunicados.

## Escenario Principal de Éxito (Flujo Básico)

1. El Afiliado accede a la pantalla principal (bandeja de entrada) de la aplicación móvil.
2. El Sistema identifica al Afiliado y consulta sus criterios de pertenencia: canales a los que está suscrito y grupos en los que está incluido por la Junta Directiva.
3. El Sistema recupera el historial de comunicados que han sido emitidos hacia esos canales y grupos específicos.
4. El Sistema unifica todos los comunicados recuperados, los ordena estrictamente en orden cronológico inverso (del más reciente al más antiguo) y muestra una lista con el resumen de cada uno (título, emisor, fecha y extracto del texto).

## Extensiones (Flujos Alternativos)

* **4a. Bandeja vacía (Sin comunicados históricos):**
    1. Tras realizar la consulta, el Sistema detecta que no existe ningún comunicado emitido para los canales o grupos del Afiliado.
    2. El Sistema muestra un indicador visual en la pantalla informando de que la bandeja está vacía ("Aún no tienes comunicados").
    3. El Caso de Uso termina.

* **4b. Paginación / Carga diferida de historial:**
    1. El Sistema detecta que el volumen histórico de comunicados es muy elevado.
    2. El Sistema muestra únicamente el bloque más reciente (ej. los últimos 20 comunicados).
    3. Cuando el Afiliado se desplaza hasta el final de la lista (*scroll*), el Sistema recupera y anexa el siguiente bloque temporal al listado de forma transparente.

## Requisitos Especiales

* **Regla de Consolidación:** La bandeja de entrada debe unificar y mezclar de forma transparente para el usuario tanto los comunicados de "Canales" (suscripción voluntaria) como los de "Grupos" (asignación administrativa), primando siempre el orden cronológico absoluto.
* **Tolerancia a fallos de red (Caché):** Si el dispositivo carece de conexión a internet al abrir la aplicación, el Sistema deberá mostrar el último listado de comunicados guardado en la caché local del dispositivo, indicando visualmente que se trata de una versión "Sin conexión" no actualizada.
