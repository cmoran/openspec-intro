## Context

La expansión a múltiples locales físicos exige transformar el modelo de stock plano a una matriz `(producto, sucursal)`. Para mantener la disciplina operativa en toda la red, se introduce control de acceso por roles (RBAC) con delimitación por sucursal, y un sistema de alerta temprana que evite quiebres de inventario no anticipados.

## Goals / Non-Goals

**Goals:**
- Desacoplar el stock del producto hacia una entidad de relación `location_stock`.
- Esquema de roles estricto (Admin, Encargado de Inventario, Vendedor) mediante claims en JWT y middlewares de autorización.
- Monitoreo en tiempo real de umbrales mínimos (`min_threshold`) con generación de alertas persistentes.
- API de reportes de quiebres con cálculo de déficit y filtrado por local.

**Non-Goals:**
- Generación automática de órdenes de compra a proveedores externos (se aborda en el siguiente cambio).
- Transferencias o despachos entre bodegas con estados de tránsito intermedio (fuera del alcance de esta fase).

## Decisions

### 1. Modelo de Stock por Ubicación
- **Decisión**: La cantidad física ya no reside en `products.current_stock`, sino en la tabla `location_stock` con clave compuesta `(product_id, location_id)`.
- **Razón**: Permite escalabilidad horizontal a cualquier número de locales sin modificar la tabla de productos.

### 2. Esquema de Base de Datos
- Tabla `locations`:
  - `id`: UUID (Primary Key)
  - `code`: VARCHAR(20) UNIQUE NOT NULL
  - `name`: VARCHAR(100) NOT NULL
  - `address`: VARCHAR(255)
  - `is_active`: BOOLEAN DEFAULT TRUE
- Tabla `location_stock`:
  - `product_id`: UUID REFERENCES products(id)
  - `location_id`: UUID REFERENCES locations(id)
  - `current_stock`: INT NOT NULL DEFAULT 0
  - `min_threshold`: INT NOT NULL DEFAULT 5
  - PRIMARY KEY (`product_id`, `location_id`)
- Tabla `users`:
  - `id`: UUID (Primary Key)
  - `email`: VARCHAR(150) UNIQUE NOT NULL
  - `password_hash`: VARCHAR(255) NOT NULL
  - `role`: ENUM('ADMINISTRATOR', 'INVENTORY_MANAGER', 'VENDOR') NOT NULL
  - `assigned_location_id`: UUID REFERENCES locations(id) (opcional si es ADMIN)
- Tabla `stock_alerts`:
  - `id`: UUID (Primary Key)
  - `product_id`: UUID REFERENCES products(id)
  - `location_id`: UUID REFERENCES locations(id)
  - `severity`: ENUM('WARNING', 'CRITICAL', 'STOCKOUT')
  - `current_stock`: INT NOT NULL
  - `min_threshold`: INT NOT NULL
  - `is_resolved`: BOOLEAN DEFAULT FALSE
  - `created_at`: TIMESTAMP DEFAULT NOW()

### 3. Mecanismo de Alertas
- Tras cada confirmación de venta o salida, un observador o servicio de eventos verifica si `current_stock <= min_threshold`.
- Si se cumple la condición, se inserta o actualiza la alerta activa en `stock_alerts` de forma idempotente para no saturar con duplicados.

## Risks / Trade-offs

- **[Complejidad en consultas multi-sucursal]** → *Mitigación*: Vista materializada o consulta agregada indexada para el consolidado global de stock de la empresa.
