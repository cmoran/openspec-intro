## Why

El sistema actual detecta y reporta quiebres de stock en las sucursales, pero la solución de reabastecimiento sigue siendo manual, lenta y propensa a descuidos: los encargados deben contactar por teléfono o email a los distribuidores y redactar pedidos sin estandarización. Para cerrar el ciclo logístico integral, este cambio introduce la gestión de proveedores, órdenes de compra formales y un mecanismo de reabastecimiento automático que genera sugerencias de compra en cuanto se dispara una alerta de quiebre.

## What Changes

- Creación del directorio y catálogo de proveedores vinculados a los productos que suministran con costos pactados y tiempos de entrega (*lead times*).
- Flujo de vida de Órdenes de Compra (PO): borrador (`DRAFT`), aprobada (`APPROVED`), enviada (`SENT`), recepcionada (`RECEIVED`) y cancelada (`CANCELLED`).
- Reabastecimiento automático: ante un evento de quiebre de stock en una sucursal, el sistema genera automáticamente un borrador de orden de compra al proveedor preferido para reponer hasta el stock objetivo.
- Módulo de recepción física de mercadería en bodega o sucursal que ingresa el stock directamente y registra el movimiento de inventario correspondiente.

## Capabilities

### New Capabilities
- `supplier-management`: Registro y administración de proveedores, datos de contacto y matriz producto-proveedor con costos de compra.
- `purchase-orders`: Gestión del ciclo de vida de órdenes de compra y aprobación administrativa.
- `automated-replenishment`: Generación automática de sugerencias de compra disparadas por alertas de quiebre de stock y recepción física de mercancía en inventario.

### Modified Capabilities
*(Ninguna)*

## Impact

- **Modelos de datos**: Nuevas tablas `suppliers`, `product_suppliers`, `purchase_orders`, `purchase_order_items`.
- **Integraciones internas**: El subsistema de alertas de stock ahora emite eventos que activan el generador de sugerencias de compra.
- **APIs**: Endpoints `/api/v1/suppliers`, `/api/v1/purchase-orders` y `/api/v1/purchase-orders/:id/receive`.
