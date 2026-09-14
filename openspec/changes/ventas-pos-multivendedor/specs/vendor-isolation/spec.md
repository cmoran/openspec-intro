## ADDED Requirements

### Requirement: Restricción de acceso a inventario para vendedores
El sistema MUST prohibir que usuarios con credenciales de vendedor ejecuten operaciones de entrada, salida manual o ajustes de inventario.

#### Scenario: Intento de ajuste de inventario por un vendedor
- **WHEN** un usuario autenticado con rol vendedor envía una solicitud al endpoint `/api/v1/inventory/adjustment` o `/api/v1/inventory/movements`
- **THEN** el sistema rechaza la petición con código HTTP 403 Forbidden registrando el intento en el log de seguridad

### Requirement: Vista segregada para ventas
El sistema MUST proveer vistas e interfaces diferenciadas, de modo que la vista de punto de venta no exponga costos de compra ni herramientas de edición de catálogo.

#### Scenario: Consulta de catálogo desde el punto de venta
- **WHEN** el punto de venta consulta el catálogo disponible para facturar
- **THEN** el sistema retorna SKU, nombre y precio de venta al público, ocultando márgenes y precios de costo
