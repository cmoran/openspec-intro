## Why

El negocio opera actualmente con planillas desconectadas y conteos informales en su tienda física, lo que ocasiona discrepancias de stock, pérdida de ventas por falta de producto no detectada y falta de trazabilidad sobre mermas y ajustes manuales. Este cambio establece la base técnica de gestión de inventario para un único local con un catálogo centralizado y un registro transaccional de movimientos (kardex).

## What Changes

- Creación del catálogo de productos con identificación única por código SKU y código de barras.
- Registro de movimientos de inventario clasificados en tres tipos: entrada (`ENTRY`), salida (`EXIT`) y ajuste por inventario físico (`ADJUSTMENT`).
- Cálculo en tiempo real del stock disponible para evitar sobreventas o descuadres.
- Historial inmutable de auditoría para cada variación de stock con motivo y fecha.

## Capabilities

### New Capabilities
- `product-catalog`: Gestión del catálogo maestro de productos (altas, bajas lógicas, edición y búsqueda por SKU).
- `inventory-movements`: Registro y consulta de movimientos de inventario con balance transaccional y kardex.

### Modified Capabilities
*(Ninguna, es la propuesta fundacional del sistema)*

## Impact

- **Modelos de datos**: Creación de las tablas `products` e `inventory_movements`.
- **APIs**: Endpoints REST para catálogo `/api/v1/products` y movimientos `/api/v1/inventory/movements`.
- **Dependencias**: Base de datos relacional (PostgreSQL / SQLite) con soporte para transacciones ACID.
