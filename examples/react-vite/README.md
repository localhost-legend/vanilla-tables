# React Vite Example

This example demonstrates how to use Vanilla Tables with React and Vite.

## Run the example

```bash
npm install
npm run dev
```

## Code

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
