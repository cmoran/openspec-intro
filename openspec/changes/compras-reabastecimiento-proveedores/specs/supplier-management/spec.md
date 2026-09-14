## ADDED Requirements

### Requirement: Registro y catálogo de proveedores
El sistema MUST registrar proveedores comerciales con razón social, RUT/identificación fiscal, email de contacto y condiciones comerciales.

#### Scenario: Alta de nuevo proveedor
- **WHEN** un usuario administrador registra un proveedor con identificación fiscal única
- **THEN** el sistema almacena al proveedor y lo habilita para vincular productos a su catálogo

### Requirement: Asociación producto-proveedor y costo pactado
El sistema MUST permitir vincular uno o más productos a un proveedor, definiendo el costo unitario de compra y el tiempo estimado de entrega en días (*lead time*).

#### Scenario: Asignación de proveedor preferido
- **WHEN** se asocia un proveedor a un producto marcándolo como `is_preferred: true`
- **THEN** el sistema utiliza este proveedor por defecto para futuros cálculos de reposición automática
