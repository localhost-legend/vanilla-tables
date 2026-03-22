# Vanilla Tables

Fast, framework-agnostic data tables in pure JavaScript.

[![CI](https://github.com/lhozdroid/vanilla-tables/actions/workflows/ci.yml/badge.svg)](https://github.com/lhozdroid/vanilla-tables/actions/workflows/ci.yml)
[![npm version](https://img.shields.io/npm/v/vanilla-tables.svg)](https://www.npmjs.com/package/vanilla-tables)
[![npm downloads](https://img.shields.io/npm/dm/vanilla-tables.svg)](https://www.npmjs.com/package/vanilla-tables)
[![GitHub Discussions](https://img.shields.io/github/discussions/lhozdroid/vanilla-tables)](https://github.com/lhozdroid/vanilla-tables/discussions)

Vanilla Tables is an object-oriented table engine designed for real apps: rich features, extensible API, production-safe behavior, and strong test coverage.

## Why Vanilla Tables

- No framework lock-in: works with plain HTML, React, Vue, Angular, Svelte, or any SSR/CSR stack.
- Feature-complete table core: search, filters, sorting, paging, fixed regions, row expansion/editing/actions.
- Built for production: worker projection with timeout/retry/fallback, resilient state flow, and release verification gates.
- Theme-ready: raw semantic output by default, plus framework-focused theme plugins.

## Install

```bash
npm install vanilla-tables
```

## Quick Start (npm)

```js
import { createVanillaTable, bootstrapThemePlugin } from 'vanilla-tables';
import 'vanilla-tables/styles';

const rows = [
    { id: 1, name: 'Alice', city: 'NYC', score: 45 },
    { id: 2, name: 'Bob', city: 'Paris', score: 20 }
];

const table = createVanillaTable(document.querySelector('#table-root'), rows, {
    pageSize: 10,
    searchable: true,
    columnFilters: true,
    multiSort: true,
    rowActions: [
        {
            id: 'approve',
            label: 'Approve',
            onClick: ({ row }) => console.log('approve', row)
        }
    ]
});

table.use(bootstrapThemePlugin());
```

## Quick Start (CDN)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/vanilla-tables/dist/vanilla-tables.css" />
<script src="https://cdn.jsdelivr.net/npm/vanilla-tables/dist/vanilla-tables.min.js"></script>
<script>
    const table = new VanillaTables.VanillaTable(document.querySelector('#table-root'), data, {
        fixedHeader: true,
        fixedFooter: true,
        fixedColumns: 1
    }).init();
</script>
```

## Framework Wrappers

Vanilla Tables is framework-agnostic. Use these minimal wrappers to integrate with your preferred stack.

Wrapper behavior note:
These minimal wrappers treat `options` as initialization-time configuration.
If `options` can change at runtime in your app, re-create the table instance when `options` change.

### React

```jsx
import React, { useEffect, useRef } from 'react';
import { createVanillaTable } from 'vanilla-tables';
import 'vanilla-tables/styles';

const VanillaTableWrapper = ({ data, options }) => {
    const tableRef = useRef(null);
    const instanceRef = useRef(null);

    useEffect(() => {
        // Mount
        if (!instanceRef.current) {
            instanceRef.current = createVanillaTable(tableRef.current, data, options);
        }
    }, [data, options]);

    useEffect(() => {
        return () => {
            // Destroy
            if (instanceRef.current) {
                instanceRef.current.destroy();
            }
        };
    }, []);

    return <div ref={tableRef} />;
};
```

### Vite Example

See [examples/react-vite](https://github.com/lhozdroid/vanilla-tables/tree/main/examples/react-vite) for a runnable React + Vite example.