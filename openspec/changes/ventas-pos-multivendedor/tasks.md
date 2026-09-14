## 1. Modelado de Datos y Migraciones

- [ ] 1.1 Crear migración para las tablas `vendors`, `sales` y `sale_items`
- [ ] 1.2 Agregar tipo `SALE` al enum de tipos de movimientos de inventario
- [ ] 1.3 Configurar índices en `sales(vendor_id, created_at)` y `sale_items(sale_id, product_id)`

## 2. Autenticación y Control de Acceso

- [ ] 2.1 Implementar servicio de autenticación para vendedores con PIN o contraseña
- [ ] 2.2 Implementar middleware de roles que restrinja el acceso a endpoints de inventario a usuarios con rol de vendedor
- [ ] 2.3 Crear endpoint `/api/v1/auth/vendor/login`

## 3. Lógica de Negocio y Transacciones de Venta

- [ ] 3.1 Implementar `SaleService` con lógica transaccional de venta
- [ ] 3.2 Implementar bloqueo pesimista en la consulta de stock para prevenir sobreventas concurrentes
- [ ] 3.3 Integrar emisión de movimiento de inventario automático de tipo `SALE`

## 4. Endpoints del Punto de Venta (POS)

- [ ] 4.1 Crear endpoint `POST /api/v1/pos/sales` para registro y confirmación de tickets
- [ ] 4.2 Crear endpoint `GET /api/v1/pos/products` (catálogo público sin costos de adquisición)
- [ ] 4.3 Crear endpoint `GET /api/v1/pos/sales/my-sales` filtrado por el vendedor autenticado

## 5. Pruebas y Validación

- [ ] 5.1 Escribir test de seguridad: verificar que un vendedor recibe `403 Forbidden` al intentar llamar a `/api/v1/inventory/adjustment`
- [ ] 5.2 Escribir test de concurrencia: dos ventas paralelas sobre el mismo ítem con stock limitado
- [ ] 5.3 Validar que el stock se descuenta exactamente en la cantidad vendida
