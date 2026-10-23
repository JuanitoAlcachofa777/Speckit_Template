# Módulo Logística e Inventario — ERP (Vite + Vanilla JS)

El presente repositorio contiene el MVP del módulo de Logística e Inventario del ERP, el cual está desarrollado por Juan Pablo Valladares, Estefano Gutiérrez, Florencia Casas, Catalina Cifuentes, Gonzalo Romero, y Martín Salas.
Se construye sobre el template base de Speckit

La visión que tenemos del producto es poder entregar una herramienta a empresas (como por ejemplo, pymes) que les permita trazar en todo momento sus existencias (stock) y hacia dónde de van moviendo, evitando así una logística llevada a cabo a lápiz y papel que acarrea distintos problemas con un entorno cada vez más digitalizado (pérdidas por malos registros, mal uso y manejo de la información). Consideramos los roles de arquitecto o administrador del módulo, que puedan acceder al software con control total, y a los usuarios (los distintos actores que juegan un papel en una bodega), para que puedan tener control sobre los flujos de los distintos stocks, supervisiones, movimientos, entre otras actividades. El MVP que se busca realizar contará con funciones de catálogo de productos, registros, movimientos, stocks por cada producto, con posibilidad de ampliar a más actividades en iteraciones futuras con los modelos de IA. El historial de uso de GenIA con los distintos prompts quedará registrado como un anexo con modelo, fecha, y secuencia de prompts.

## Setup

Install dependencies and run the dev server:

```bash
npm install
npm run dev
```

Open http://localhost:5173 (or the URL Vite prints).

## Tests

Run the full test suite (unit + integration) with:

```bash
npm test
```

Or a one-off run without watch mode:

```bash
npx vitest run
```

## Building Custom Modules

Custom ERP modules built on this base define:

- What an **Entity** represents (e.g., customer, product, asset) plus domain-specific fields.
- What a **Document** represents (e.g., order, invoice, contract).
- Module-specific labels and validation rules.

The base module handles identity generation, timestamps, referential integrity between entities and documents, cascade deletion, navigation, and search. See `specs/001-erp-base-module/` for the full specification.

## Tailoring via Configuration

All tailoring lives in one place: the `userConfig` object at the top of `src/module-config.js`. Developers edit this single export; no core code changes are required. Available knobs:

- `moduleName` — display name shown in the top bar, dashboard, and document title; also derives the logo monogram.
- `labels` — rename Entity/Document terms across every screen, including sidebar buttons, quick actions, forms, and messages. Spanish articles and agreements (el/la, seleccionado/a) are derived from the first word of each configured term.
- `customFields` — declare extra fields per record type with `target` (`entity` or `document`), `key`, `label`, `type` (`text`, `number`, `date`, or `select`), optional `required`, and `options` for selects. Keys must not collide with base field names (see `specs/001-erp-base-module/data-model.md`). Values persist alongside records and appear in search results.
- `statusLifecycle` — restrict document statuses to a configured list of `statuses` with permitted `transitions` between them. Without it, status stays a free-form field.
- `theme.accentColor` — a hex color (e.g., `"#0f62fe"`) applied across the shell as highlights and primary controls. Invalid values fall back to the default.

If the configuration is invalid (unknown types, bad keys, transitions referencing unknown statuses, non-hex accent colors), the app shows the problems in a banner and falls back to defaults instead of breaking.

## Notes

- Data is persisted to `localStorage` under the keys `erp_base_module:entities` and `erp_base_module:documents`.
- Deleting an entity also deletes its documents (cascade).
- This project intentionally uses vanilla HTML, CSS, and JavaScript for simplicity.
