## ADDED Requirements

### Requirement: Control de acceso basado en roles
El sistema MUST implementar autenticación de usuarios con asignación de uno de los siguientes roles: ADMINISTRATOR, INVENTORY_MANAGER o VENDOR.

#### Scenario: Acceso de administrador
- **WHEN** un usuario con rol `ADMINISTRATOR` accede a cualquier módulo del sistema
- **THEN** el sistema autoriza todas las operaciones de configuración, usuarios, inventario y reportes

#### Scenario: Acceso de encargado de inventario
- **WHEN** un usuario con rol `INVENTORY_MANAGER` intenta registrar entradas, salidas o ajustes de inventario
- **THEN** la operación es autorizada, pero se le deniega el acceso a la creación de usuarios o cambios de configuración global

#### Scenario: Vendedor intentando operar fuera de su sucursal asignada
- **WHEN** un usuario con rol `VENDOR` asignado a la sucursal `SUC-CENTRO` intenta emitir una venta para la sucursal `SUC-SUR`
- **THEN** el sistema rechaza la petición con código HTTP 403 Forbidden
