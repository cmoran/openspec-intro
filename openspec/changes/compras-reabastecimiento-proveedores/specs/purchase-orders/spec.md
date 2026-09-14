## ADDED Requirements

### Requirement: Gestión del ciclo de vida de la orden de compra
El sistema MUST gestionar órdenes de compra a través de estados controlados: DRAFT, APPROVED, SENT, RECEIVED y CANCELLED.

#### Scenario: Aprobación de orden de compra
- **WHEN** un administrador aprueba una orden en estado `DRAFT`
- **THEN** la orden cambia al estado `APPROVED` quedando lista para envío formal al proveedor

#### Scenario: Recepción física con incremento de inventario
- **WHEN** un encargado de inventario confirma la recepción de una orden de compra en su sucursal de destino
- **THEN** el sistema cambia el estado a `RECEIVED`, incrementa automáticamente el stock de cada producto en la sucursal y genera los movimientos de inventario tipo `ENTRY` con motivo `PURCHASE_RECEIPT`
