# Especificación de Vista: [Nombre de la Vista]

**Referencia al Caso de Uso:** [Ej: CU-AD04: Gestionar Canales]

## 1. Ruta y Navegación

* **Ruta TanStack Router:** `/ruta/de/la/vista`
* **Breadcrumbs:** Inicio > Sección > Vista
* **Parámetros de URL (`Search Params` o `Path Params`):**
  * Ej: `?page=1&sort=name`
  * Ej: `/canales/$canalId`

## 2. Origen de Datos (Endpoints OpenAPI)

Mapeo exacto de qué endpoints alimentan esta vista. Prohibido usar endpoints no definidos aquí.

* **Query (Lectura):**
  * `GET /api/v1/...` -> `useQuery({ queryKey: [...], queryFn: ... })`
* **Mutations (Escritura):**
  * `POST /api/v1/...` -> `useMutation(...)`
  * `DELETE /api/v1/...`

## 3. Árbol de Componentes Esperado

Define cómo se debe trocear la vista. Deben utilizarse componentes de `shadcn/ui` siempre que sea posible.

```text
[ViewLayout]
 ├── [PageHeader] (Título y Breadcrumbs)
 ├── [ActionsBar] (Botones principales, ej. "Nuevo Canal")
 └── [DataTable] (Tabla de datos usando TanStack Table)
      ├── [Filters]
      ├── [Pagination]
      └── [RowActions] (Editar, Eliminar, etc.)
```

## 4. Esquema Lógico (Wireframe ASCII)

Esbozo de la disposición geométrica de la vista (Layout).

```text
+---------------------------------------------------------+
| [Header Global] (SIDI Admin - Avatar)                   |
+---+-----------------------------------------------------+
| S | Breadcrumbs > ...                                   |
| i |                                                     |
| d | Título de la Página                 [Botón Primario]|
| e |                                                     |
| b | +-------------------------------------------------+ |
| a | | [Filtro 1] [Filtro 2]               [Buscar...] | |
| r | |-------------------------------------------------| |
|   | | Columna 1 | Columna 2 | Columna 3 | Acciones    | |
|   | |-------------------------------------------------| |
|   | | Dato 1.1  | Dato 1.2  | Dato 1.3  | [...]       | |
|   | | Dato 2.1  | Dato 2.2  | Dato 2.3  | [...]       | |
|   | +-------------------------------------------------+ |
|   |   Pag. 1 de N                             [<] [>]   |
+---+-----------------------------------------------------+
```

## 5. Reglas y Validaciones Específicas del Cliente

* **Formularios:** (Mapear a Zod). Ej: El campo nombre no puede exceder 50 caracteres.
* **Feedback Visual:** Ej: Mostrar un Toast de éxito al guardar. Mostrar un Modal de confirmación antes de eliminar.
