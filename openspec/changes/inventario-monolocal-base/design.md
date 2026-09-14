## Context

El sistema de control de inventario monolocal es el cimiento de la plataforma comercial. Debe soportar operaciones atómicas de stock para una tienda física única, evitando sobreventas y asegurando que cada movimiento quede registrado con fines contables y de auditoría.

## Goals / Non-Goals

**Goals:**
- Modelo de datos consistente para productos y movimientos de inventario.
- Transacciones ACID a nivel de base de datos para garantizar que el saldo nunca quede en estado inconsistente.
- API REST desacoplada que sirva de base para futuras interfaces (web o POS).
- Trazabilidad total de mermas y ajustes físicos.

**Non-Goals:**
- Soporte para múltiples sucursales (se abordará en un cambio posterior).
- Módulo de punto de venta (POS) y asignación a vendedores (se abordará en el siguiente cambio).
- Autenticación compleja por roles (inicialmente acceso de operador único).

## Decisions

### 1. Modelo de Doble Registro: Saldo Actual + Kardex de Eventos
- **Decisión**: Mantener una columna `current_stock` en la tabla `products` para lecturas ultrarrápidas, pero respaldada por la tabla inmutable `inventory_movements`.
- **Alternativa descartada**: Calcular el saldo en cada consulta haciendo `SUM(movements)` (muy costoso cuando hay miles de registros).
- **Control de consistencia**: Cada actualización de `current_stock` se realiza dentro de una transacción con bloqueo de fila (`SELECT ... FOR UPDATE`) junto al `INSERT` en `inventory_movements`.

### 2. Esquema de Base de Datos
- Tabla `products`:
  - `id`: UUID (Primary Key)
  - `sku`: VARCHAR(50) UNIQUE NOT NULL
  - `name`: VARCHAR(255) NOT NULL
  - `description`: TEXT
  - `cost_price`: DECIMAL(12,2) NOT NULL
  - `sale_price`: DECIMAL(12,2) NOT NULL
  - `current_stock`: INT NOT NULL DEFAULT 0
  - `is_active`: BOOLEAN DEFAULT TRUE
  - `created_at`, `updated_at`: TIMESTAMP
- Tabla `inventory_movements`:
  - `id`: UUID (Primary Key)
  - `product_id`: UUID REFERENCES products(id)
  - `type`: ENUM('ENTRY', 'EXIT', 'ADJUSTMENT')
  - `quantity`: INT NOT NULL
  - `previous_stock`: INT NOT NULL
  - `new_stock`: INT NOT NULL
  - `reason`: VARCHAR(255) NOT NULL
  - `created_at`: TIMESTAMP DEFAULT NOW()

### 3. Stack Tecnológico
- Backend: TypeScript / Node.js con Fastify o Express.
- ORM/Query: Prisma o SQL nativo.
- Base de datos: PostgreSQL (con opción de SQLite en memoria para tests unitarios).

## Risks / Trade-offs

- **[Riesgo de concurrencia en salidas simultáneas]** → *Mitigación*: Uso estricto de transacciones con bloqueo pesimista a nivel de fila (`SELECT FOR UPDATE`) para que una salida no pueda dejar el stock en negativo.
- **[Crecimiento rápido de la tabla de movimientos]** → *Mitigación*: Índices sobre `(product_id, created_at)` para mantener las consultas de kardex optimizadas.
