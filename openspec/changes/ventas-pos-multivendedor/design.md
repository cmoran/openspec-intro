## Context

Con la incorporación de vendedores de salón, es imprescindible separar la operativa de ventas de la administración del inventario. Los vendedores necesitan un flujo de venta rápido y a prueba de errores, mientras que el inventario debe mantenerse blindado contra alteraciones no autorizadas.

## Goals / Non-Goals

**Goals:**
- Punto de venta (POS) ágil con validación de existencias en tiempo real.
- Bloqueo pesimista (`SELECT FOR UPDATE`) para garantizar consistencia cuando varios vendedores operan sobre los mismos productos.
- Restricción estricta mediante middleware de autenticación (JWT) para aislar las rutas de administración de inventario.
- Registro automático de movimientos de inventario de tipo `SALE` vinculados al ticket de venta.

**Non-Goals:**
- Facturación electrónica con entidades fiscales externas (fuera de alcance en esta fase).
- Gestión de inventario en múltiples tiendas (se aborda en el cambio siguiente).

## Decisions

### 1. Concurrencia y Control de Stock Atómico
- **Decisión**: La operación de venta adquiere un bloqueo pesimista sobre las filas de la tabla `products` involucradas antes de verificar y descontar stock.
- **Alternativa descartada**: Descuento asíncrono o eventual (genera riesgo inaceptable de sobreventa cuando el stock es bajo).

### 2. Esquema de Base de Datos
- Tabla `vendors`:
  - `id`: UUID (Primary Key)
  - `name`: VARCHAR(100) NOT NULL
  - `email`: VARCHAR(100) UNIQUE NOT NULL
  - `pin_code_hash`: VARCHAR(255) NOT NULL
  - `is_active`: BOOLEAN DEFAULT TRUE
- Tabla `sales`:
  - `id`: UUID (Primary Key)
  - `ticket_number`: VARCHAR(50) UNIQUE NOT NULL
  - `vendor_id`: UUID REFERENCES vendors(id)
  - `total_amount`: DECIMAL(12,2) NOT NULL
  - `payment_method`: ENUM('CASH', 'CARD', 'TRANSFER')
  - `status`: ENUM('COMPLETED', 'CANCELLED')
  - `created_at`: TIMESTAMP DEFAULT NOW()
- Tabla `sale_items`:
  - `id`: UUID (Primary Key)
  - `sale_id`: UUID REFERENCES sales(id)
  - `product_id`: UUID REFERENCES products(id)
  - `quantity`: INT NOT NULL
  - `unit_price`: DECIMAL(12,2) NOT NULL
  - `subtotal`: DECIMAL(12,2) NOT NULL

### 3. Seguridad y Segregación
- El token JWT emitido al vendedor contiene el claim `role: 'vendor'`.
- El middleware de la API deniega cualquier acceso a rutas bajo `/api/v1/inventory/*` si el rol no es administrativo.

## Risks / Trade-offs

- **[Riesgo de cuellos de botella por bloqueos en productos de alta demanda]** → *Mitigación*: Mantener las transacciones de venta mínimas y optimizadas en tiempo (solo lectura con bloqueo, verificación de stock, inserción de venta e inserción de movimiento).
