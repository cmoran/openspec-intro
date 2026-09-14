## ADDED Requirements

### Requirement: Emisión de venta en punto de atención
El sistema MUST registrar una nueva venta asociando el identificador del vendedor autenticado, la lista de productos con sus cantidades, el medio de pago utilizado y el total calculado.

#### Scenario: Venta exitosa con existencias disponibles
- **WHEN** el vendedor confirma una venta donde todos los ítems tienen cantidad disponible mayor o igual a la solicitada
- **THEN** el sistema persiste la venta en estado `COMPLETED`, genera el comprobante y emite evento para descontar inventario

#### Scenario: Venta rechazada por stock insuficiente
- **WHEN** un vendedor intenta vender una cantidad superior al stock físico actual de un producto
- **THEN** el sistema rechaza la transacción retornando código HTTP 422 con detalle del ítem sin stock disponible

### Requirement: Consulta de ventas por vendedor
El vendedor MUST poder consultar el historial y resumen diario de las ventas registradas durante su turno de trabajo.

#### Scenario: Consulta de ventas propias del día
- **WHEN** el vendedor solicita su lista de ventas realizadas en la jornada
- **THEN** el sistema devuelve únicamente las ventas asociadas a su propio identificador de vendedor
