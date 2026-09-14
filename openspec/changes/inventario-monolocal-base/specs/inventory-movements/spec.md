## ADDED Requirements

### Requirement: Registro de movimientos de inventario
El sistema MUST registrar movimientos de inventario asociados a un producto existente, clasificándolos en ENTRY (entrada), EXIT (salida) y ADJUSTMENT (ajuste físico), manteniendo un kardex cronológico.

#### Scenario: Entrada de mercadería por recepción
- **WHEN** se registra un movimiento de tipo `ENTRY` con cantidad mayor a cero y motivo
- **THEN** el sistema incrementa el stock actual del producto en esa cantidad y registra la entrada con timestamp en el kardex

#### Scenario: Salida de mercadería
- **WHEN** se registra un movimiento de tipo `EXIT` con cantidad menor o igual al stock disponible
- **THEN** el sistema descuenta la cantidad del stock disponible y registra el movimiento de salida

#### Scenario: Rechazo de salida por stock insuficiente
- **WHEN** se intenta registrar una salida con cantidad superior a la existencia física disponible
- **THEN** el sistema rechaza la transacción con código HTTP 422 Unprocessable Entity impidiendo saldos negativos

### Requirement: Ajuste manual de inventario con justificación
El sistema MUST permitir ajustar el stock a un valor objetivo tras un conteo físico o detección de mermas, exigiendo una justificación textual obligatoria.

#### Scenario: Ajuste por merma justificada
- **WHEN** el usuario envía un nuevo recuento físico con una justificación válida
- **THEN** el sistema calcula la diferencia, actualiza el saldo y genera un registro de tipo `ADJUSTMENT` vinculando la justificación
