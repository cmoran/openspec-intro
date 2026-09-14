## ADDED Requirements

### Requirement: Generación automática de borrador de reposición
El sistema MUST generar automáticamente una sugerencia u orden de compra en estado `DRAFT` cuando un producto entra en quiebre de stock y cuenta con un proveedor preferido asignado.

#### Scenario: Creación de sugerencia ante alerta de quiebre
- **WHEN** se dispara un evento de quiebre de stock para un producto con proveedor preferido
- **THEN** el sistema crea o agrupa en una orden de compra `DRAFT` la cantidad requerida para alcanzar el stock objetivo

#### Scenario: Producto sin proveedor configurado
- **WHEN** se produce un quiebre de stock de un producto que no tiene ningún proveedor asociado
- **THEN** el sistema emite una notificación de "Proveedor no asignado" para que el administrador asigne uno manualmente
