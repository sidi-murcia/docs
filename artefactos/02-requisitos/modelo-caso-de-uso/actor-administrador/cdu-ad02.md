# CU-AD02: Consultar Panel de Control (Dashboard)

**Actor Principal:** Administrador

## Precondiciones

1. El Administrador debe tener una sesión web activa y válida en el Panel de Administración.

## Postcondiciones

1. El sistema presenta al administrador una visión inmediata y actualizada de la actividad del sistema mediante métricas y gráficas.

## Escenario Principal de Éxito (Flujo Básico)

1. El Administrador accede a la vista principal "Dashboard" seleccionándola en el Menú de Navegación, o es redirigido automáticamente a ella tras iniciar sesión con éxito.
2. El Sistema consulta la base de datos para recuperar el registro de actividad de los últimos 7 días naturales.
3. El Sistema procesa la información para obtener datos agregados del volumen de comunicados enviados, desglosándolos por día y por canal de destino.
4. El Sistema muestra en pantalla un indicador numérico que refleja el total exacto de comunicados publicados en los últimos 7 días naturales.
5. El Sistema renderiza una gráfica de barras que representa la evolución diaria de los envíos durante la semana actual.
6. El Sistema aplica una diferenciación por colores en la gráfica de barras para identificar claramente los envíos realizados a cada canal específico.
7. El Administrador revisa las métricas presentadas.
8. El Caso de Uso termina.

## Extensiones (Flujos Alternativos)

* **2a. Ausencia de actividad en el periodo evaluado:**
    1. El Sistema detecta que no se ha publicado ningún comunicado durante los últimos 7 días naturales.
    2. El Sistema muestra el indicador numérico principal con un valor de cero (0).
    3. El Sistema renderiza la estructura de la gráfica de barras sin datos, o muestra un estado vacío (*empty state*) indicando visualmente que no hay actividad reciente para representar.
    4. El Caso de Uso retorna al paso 7 del flujo básico.

## Requisitos Especiales

* **Cálculo en tiempo real:** Para proporcionar a la Junta Directiva una visión inmediata de la actividad, los datos de la gráfica y el indicador numérico deben calcularse de forma dinámica en el momento en que se carga la vista, garantizando que incluyan cualquier comunicado que haya sido enviado instantes antes de la consulta.