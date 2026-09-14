## 1. Modelado de Datos y Migraciones

- [ ] 1.1 Crear migración para las tablas `suppliers` y `product_suppliers`
- [ ] 1.2 Crear migración para `purchase_orders` y `purchase_order_items` con estados del ciclo de vida
- [ ] 1.3 Configurar índices en `product_suppliers(product_id, is_preferred)` y `purchase_orders(status, destination_location_id)`

## 2. Gestión de Proveedores

- [ ] 2.1 Implementar `SupplierService` con endpoints CRUD para administración de proveedores
- [ ] 2.2 Endpoint para asociar productos a un proveedor con precio de costo y tiempo de entrega
- [ ] 2.3 Lógica para definir y alternar el proveedor preferido de cada producto

## 3. Generación Automática de Reposición

- [ ] 3.1 Implementar `ReplenishmentService` suscrito a eventos de quiebre de stock
- [ ] 3.2 Lógica de cálculo de cantidad a reponer: `(stock_maximo - stock_actual)`
- [ ] 3.3 Agrupación de productos en una misma orden de compra en estado `DRAFT` por proveedor y sucursal

## 4. Flujo de Órdenes de Compra y Recepción

- [ ] 4.1 Endpoints de gestión de órdenes de compra: listado, detalle y aprobación por Administrador
- [ ] 4.2 Endpoint `POST /api/v1/purchase-orders/:id/receive` para recepción física en sucursal
- [ ] 4.3 Actualización transaccional de existencias en `location_stock` e inserción en kardex de inventario
- [ ] 4.4 Cierre automático de alertas de quiebre de stock al recepcionar mercadería

## 5. Pruebas y Validación Integral

- [ ] 5.1 Prueba unitaria: cálculo correcto de lote de reposición ante evento de quiebre
- [ ] 5.2 Prueba de integración: flujo completo desde alerta de quiebre ➔ borrador generado ➔ aprobación ➔ recepción física ➔ stock actualizado
- [ ] 5.3 Validar que los escenarios de `specs/` se cumplan sin errores
