## Context

El circuito logístico se completa conectando la detección de quiebres de stock con el proceso de compras y reabastecimiento. Este módulo automatiza la generación de pedidos sugeridos a distribuidores y actualiza el inventario físico al momento en que la mercadería arriba a la sucursal.

## Goals / Non-Goals

**Goals:**
- Modelo relacional para proveedores, catálogo de compras y órdenes de compra con detalle de ítems.
- Generador automático de borradores de orden de compra ante alertas de quiebre (Stockout Events).
- Flujo de recepción que actualiza atómicamente el stock en `location_stock` e inserta los movimientos de entrada.
- Trazabilidad del estado de las compras desde el borrador hasta la recepción en almacén.

**Non-Goals:**
- Integración bancaria para pago directo de facturas (gestión de tesorería fuera de alcance).
- EDI (Electronic Data Interchange) con distribuidores multinacionales.

## Decisions

### 1. Desacoplamiento por Eventos de Quiebre
- **Decisión**: El módulo de reabastecimiento escucha los eventos emitidos por `stockout-alerts`. Cuando una alerta de quiebre se emite, el servicio de compras busca o crea una orden `DRAFT` dirigida al proveedor preferido de ese producto.
- **Alternativa descartada**: Consultar periódicamente la base de datos con un cron job (menos reactivo y genera consultas redundantes).

### 2. Esquema de Base de Datos
- Tabla `suppliers`:
  - `id`: UUID (Primary Key)
  - `tax_id`: VARCHAR(30) UNIQUE NOT NULL
  - `name`: VARCHAR(150) NOT NULL
  - `contact_email`: VARCHAR(100) NOT NULL
  - `phone`: VARCHAR(50)
  - `is_active`: BOOLEAN DEFAULT TRUE
- Tabla `product_suppliers`:
  - `product_id`: UUID REFERENCES products(id)
  - `supplier_id`: UUID REFERENCES suppliers(id)
  - `cost_price`: DECIMAL(12,2) NOT NULL
  - `lead_time_days`: INT DEFAULT 3
  - `is_preferred`: BOOLEAN DEFAULT FALSE
  - PRIMARY KEY (`product_id`, `supplier_id`)
- Tabla `purchase_orders`:
  - `id`: UUID (Primary Key)
  - `order_number`: VARCHAR(50) UNIQUE NOT NULL
  - `supplier_id`: UUID REFERENCES suppliers(id)
  - `destination_location_id`: UUID REFERENCES locations(id)
  - `status`: ENUM('DRAFT', 'APPROVED', 'SENT', 'RECEIVED', 'CANCELLED') DEFAULT 'DRAFT'
  - `total_cost`: DECIMAL(12,2) NOT NULL DEFAULT 0
  - `created_at`, `updated_at`: TIMESTAMP
- Tabla `purchase_order_items`:
  - `id`: UUID (Primary Key)
  - `purchase_order_id`: UUID REFERENCES purchase_orders(id)
  - `product_id`: UUID REFERENCES products(id)
  - `quantity`: INT NOT NULL
  - `unit_cost`: DECIMAL(12,2) NOT NULL
  - `subtotal`: DECIMAL(12,2) NOT NULL

### 3. Recepción Transaccional
- Al recibir una orden (`POST /api/v1/purchase-orders/:id/receive`), se ejecuta una transacción única que:
  1. Marca la orden como `RECEIVED`.
  2. Incrementa `location_stock.current_stock` en la sucursal de destino.
  3. Crea un registro en `inventory_movements` (tipo `ENTRY`, motivo `PURCHASE_RECEIPT`).
  4. Resuelve (`is_resolved = true`) las alertas de quiebre activas para esos productos en esa sucursal.

## Risks / Trade-offs

- **[Riesgo de generar órdenes duplicadas si se emiten múltiples alertas]** → *Mitigación*: Agrupar líneas dentro de un borrador de orden existente para el mismo proveedor y sucursal si dicho borrador aún no ha sido aprobado ni enviado.
