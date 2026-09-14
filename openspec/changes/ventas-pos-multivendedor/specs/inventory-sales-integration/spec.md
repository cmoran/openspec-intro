## ADDED Requirements

### Requirement: Descuento atómico de existencias por venta
El sistema MUST descontar las unidades vendidas directamente del inventario dentro de la misma transacción de base de datos en que se genera la venta.

#### Scenario: Descuento atómico tras venta completada
- **WHEN** se aprueba la venta de 3 unidades del SKU `PROD-001`
- **THEN** el stock del producto disminuye en 3 unidades y se genera automáticamente un movimiento de tipo `SALE` en el kardex

#### Scenario: Venta concurrente sobre última unidad
- **WHEN** dos vendedores intentan confirmar simultáneamente la venta de la última unidad disponible del mismo producto
- **THEN** solo una de las transacciones tiene éxito y la segunda es rechazada por falta de existencias
