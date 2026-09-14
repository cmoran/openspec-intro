## 1. Migraciones y Modelo de Datos

- [ ] 1.1 Crear migración para las tablas `locations` y `location_stock`
- [ ] 1.2 Crear migración para la tabla `users` con roles (ADMINISTRATOR, INVENTORY_MANAGER, VENDOR) y asignación de local
- [ ] 1.3 Crear migración para la tabla `stock_alerts` con índices en `(location_id, is_resolved)`

## 2. Autenticación y Autorización RBAC

- [ ] 2.1 Implementar servicio de autenticación unificada con JWT incluyendo rol y `assigned_location_id`
- [ ] 2.2 Crear middleware de verificación de permisos por ruta y validación de sucursal de operación
- [ ] 2.3 Endpoints CRUD de gestión de usuarios (exclusivo para rol ADMINISTRATOR)

## 3. Lógica de Stock por Ubicación

- [ ] 3.1 Refactorizar `InventoryService` para exigir `location_id` en todas las operaciones de entrada, salida y venta
- [ ] 3.2 Migrar datos de existencias existentes hacia la sucursal principal por defecto
- [ ] 3.3 Endpoints para consulta de stock por local y consolidado general

## 4. Sistema de Alertas y Reportes de Quiebre

- [ ] 4.1 Implementar detector automático de quiebre de stock (`StockoutWatcher`)
- [ ] 4.2 Crear endpoint `GET /api/v1/reports/stockouts` con filtros por sucursal y severidad
- [ ] 4.3 Endpoint para resolver o silenciar alertas cuando el stock es repuesto

## 5. Pruebas y Validación

- [ ] 5.1 Pruebas de permisos: validar que un vendedor no puede operar en una sucursal ajena
- [ ] 5.2 Pruebas de alertas: verificar que una venta que cruza el umbral genera la alerta con la severidad adecuada
- [ ] 5.3 Pruebas del reporte de quiebres comparando saldos reales vs umbrales mínimos
