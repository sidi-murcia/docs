# SIDI - Documentación del Sistema

Este repositorio centraliza la ingeniería de requisitos, el modelado y el diseño de arquitectura del **Sistema de Difusión Sindical (SIDI)**. 

Toda la documentación se rige por las disciplinas del **Proceso Unificado**, adaptándose a una estructura simplificada bajo el enfoque de documentación como código (*Docs-as-Code*).

## 🗂️ Estructura del Repositorio

El árbol de directorios se divide estrictamente en dos grandes bloques:

* **`Artefactos/`**: Contiene todos los documentos vivos de texto plano (`.md`) y esquemas que conforman las disciplinas del Proceso Unificado.
  * `ModeladoDelNegocio/`: Reglas de negocio y modelo de dominio.
  * `Requisitos/`: Especificación Complementaria (SRS), Glosario y Casos de Uso detallados por actor.
  * `Diseno/`: Arquitectura de software, esquema relacional de la base de datos y contratos de la API (OpenAPI).
* **`Recursos/`**: Almacena los ficheros binarios de referencia, bibliografía de apoyo (PDFs) y los ficheros fuente de los diagramas visuales (como los ficheros `.asta`).

## 📖 Edición y Mantenimiento

* **Documentos y Contratos:** Los textos y especificaciones están redactados en Markdown, facilitando su control de versiones y la lectura directa en el editor o en GitHub.
* **Modelado Visual:** Los diagramas estáticos se gestionan mediante fuentes editables (ej. Astah) ubicadas en el directorio de *Recursos*, exportando las vistas resultantes hacia los directorios de *Artefactos* correspondientes.

## 🔗 Enlaces a Componentes de Software

* [`sidi-murcia/backend`](https://github.com/sidi-murcia/backend) - Lógica de servidor y servicios.
* [`sidi-murcia/admin-web`](https://github.com/sidi-murcia/-web) - Panel de administración web.
* [`sidi-murcia/app`](https://github.com/sidi-murcia/app) - Aplicación móvil para afiliados.
