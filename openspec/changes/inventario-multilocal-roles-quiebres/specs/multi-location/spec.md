## ADDED Requirements

### Requirement: Gestión de sucursales y bodegas
El sistema MUST permitir el registro de sucursales físicas identificadas por un código único, nombre, dirección y estado operativo.

#### Scenario: Alta de nueva sucursal
- **WHEN** un administrador registra una nueva sucursal con código único
- **THEN** la sucursal se crea con estado activo y queda disponible para asignación de stock

### Requirement: Existencias aisladas por sucursal
El sistema MUST mantener el saldo de stock de cada producto segmentado por sucursal física mediante registros de inventario independientes.

#### Scenario: Consulta de stock en sucursal específica
- **WHEN** se consulta el stock del SKU `PROD-001` filtrando por la sucursal `SUC-CENTRO`
- **THEN** el sistema retorna únicamente las unidades físicas disponibles en esa ubicación

#### Scenario: Descuento de stock en la sucursal correspondiente
- **WHEN** se concreta una venta en la sucursal `SUC-NORTE`
- **THEN** el stock disminuye únicamente en el inventario de `SUC-NORTE` sin afectar las demás sucursales
