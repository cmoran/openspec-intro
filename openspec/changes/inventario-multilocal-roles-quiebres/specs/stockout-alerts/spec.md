## ADDED Requirements

### Requirement: Generación automática de alerta de quiebre de stock
El sistema MUST generar una alerta de quiebre de stock inmediatamente después de que un movimiento deje las existencias de un producto en un local menor o igual al umbral mínimo configurado (`min_threshold`).

#### Scenario: Detección automática al alcanzar umbral crítico
- **WHEN** una venta o ajuste deja el stock del producto en 3 unidades y el umbral mínimo para ese local es 5
- **THEN** el sistema crea una alerta activa clasificada con severidad `WARNING` o `CRITICAL`

#### Scenario: Stock en cero unidades
- **WHEN** las existencias de un producto en una sucursal llegan a 0 unidades
- **THEN** la alerta se marca como `STOCKOUT` con la máxima prioridad para atención inmediata

### Requirement: Reporte consolidado de quiebres de stock
El sistema MUST proporcionar un reporte consultable y exportable de todos los productos en quiebre o riesgo de quiebre, filtrable por sucursal.

#### Scenario: Consulta de reporte por sucursal
- **WHEN** un administrador o encargado de inventario solicita el reporte de quiebres para la sucursal `SUC-CENTRO`
- **THEN** el sistema retorna la lista ordenada de productos con stock actual, umbral mínimo y déficit de unidades
