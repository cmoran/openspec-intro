## Why

El local comercial ha incorporado múltiples vendedores para atención al público y necesita registrar transacciones de venta de forma ágil y segura. Actualmente no existe un punto de venta (POS) y permitir que los vendedores alteren manualmente el inventario provocaría pérdidas de trazabilidad, errores humanos y riesgos de fraude. Este cambio introduce un subsistema de ventas aislado donde los vendedores solo pueden operar tickets de venta, y el stock se descuenta de forma atómica y automática.

## What Changes

- Creación del subsistema de Punto de Venta (POS) con interfaz y endpoints dedicados a la emisión de tickets de venta.
- Segregación estricta de responsabilidades: los usuarios con rol vendedor no tienen acceso a endpoints ni vistas de ajuste o modificación manual de inventario.
- Integración transaccional: cada venta confirmada descuenta inmediatamente las existencias de los productos vendidos registrando un movimiento de tipo venta.
- Bloqueo de ventas cuando la cantidad solicitada excede el stock disponible en tienda.

## Capabilities

### New Capabilities
- `sales-pos`: Registro de ventas, cálculo de totales, medios de pago y emisión de comprobantes.
- `vendor-isolation`: Políticas de control de acceso y separación de interfaces para que los vendedores solo puedan registrar y consultar sus propias ventas.
- `inventory-sales-integration`: Descuento automático y atómico de stock originado por transacciones de venta completadas.

### Modified Capabilities
*(Ninguna)*

## Impact

- **Modelos de datos**: Nuevas tablas `sales`, `sale_items`, `vendors`.
- **APIs**: Nuevos endpoints `/api/v1/pos/sales`, `/api/v1/pos/products` (catálogo solo lectura sin costos).
- **Seguridad**: Middleware de autorización para restringir llamadas a `/api/v1/inventory/*` impidiendo acceso a vendedores.
