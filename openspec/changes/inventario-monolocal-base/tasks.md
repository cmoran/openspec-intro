## 1. Modelado y Base de Datos

- [ ] 1.1 Crear migración para la tabla `products` con campos SKU, precios y stock actual
- [ ] 1.2 Crear migración para la tabla `inventory_movements` con tipos de movimiento y claves foráneas
- [ ] 1.3 Configurar índices en `products(sku)` e `inventory_movements(product_id, created_at)`

## 2. Servicios de Negocio

- [ ] 2.1 Implementar `ProductService` con métodos de alta, validación de SKU único y búsqueda
- [ ] 2.2 Implementar `InventoryService` con operaciones transaccionales para ENTRY, EXIT y ADJUSTMENT
- [ ] 2.3 Implementar validación para impedir que el stock disponible sea negativo

## 3. Endpoints de la API

- [ ] 3.1 Crear endpoints REST `/api/v1/products` (POST, GET, GET por SKU)
- [ ] 3.2 Crear endpoints REST `/api/v1/inventory/movements` (POST registrar, GET kardex histórico)
- [ ] 3.3 Crear endpoint `/api/v1/inventory/adjustment` para ajustes físicos justificados

## 4. Pruebas y Validación

- [ ] 4.1 Escribir pruebas unitarias para validación de SKU duplicado y cálculos de stock
- [ ] 4.2 Escribir pruebas de integración para verificar transacciones y bloqueos de fila
- [ ] 4.3 Validar que el flujo completo cumpla los escenarios descritos en `specs/`
