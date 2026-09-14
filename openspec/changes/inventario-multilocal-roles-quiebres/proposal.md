## Why

El negocio se ha expandido a múltiples sucursales y bodegas físicas. Manejar un único inventario global ya no es viable porque cada local tiene su propio espacio físico, ventas independientes y necesidades particulares de reposición. Además, se requiere un esquema formal de permisos por roles (administrador, encargado de inventario y vendedor) y un sistema proactivo que detecte quiebres de stock antes de que los productos se agoten por completo en las tiendas.

## What Changes

- Soporte para múltiples locales o sucursales físicas (`locations`), vinculando el stock de cada producto a cada local de forma independiente.
- Sistema de autenticación y autorización basado en roles (RBAC):
  - **Administrador**: Gestión global de sucursales, usuarios, reportes y configuración.
  - **Encargado de Inventario**: Entradas, salidas y ajustes de stock en los locales autorizados.
  - **Vendedor**: Solo emisión de ventas en la sucursal asignada durante su turno.
- Motor de detección de quiebres de stock: alerta en tiempo real cuando el stock de un producto en un local alcanza o baja del nivel mínimo definido (`min_threshold`).
- Módulo de reportes de quiebres y productos críticos filtrable por sucursal.

## Capabilities

### New Capabilities
- `multi-location`: Registro de sucursales y desglose de existencias por local.
- `rbac-auth`: Gestión de identidades, asignación de roles y control de acceso granular por ubicación.
- `stockout-alerts`: Cálculo y generación de alertas de quiebre de stock y exportación de reportes de riesgo de desabastecimiento.

### Modified Capabilities
*(Ninguna)*

## Impact

- **Modelos de datos**: Nuevas tablas `locations`, `location_stock`, `users`, `roles`, `stock_alerts`.
- **APIs**: Rutas de gestión de sucursales `/api/v1/locations`, usuarios `/api/v1/users`, y reportes de quiebre `/api/v1/reports/stockouts`.
- **Lógica de ventas**: La venta ahora debe obligatoriamente indicar o derivar el `location_id` del vendedor.
