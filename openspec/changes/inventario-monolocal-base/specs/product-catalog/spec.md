## ADDED Requirements

### Requirement: Registro y alta de producto
El sistema MUST permitir registrar nuevos productos con código SKU único, nombre, descripción opcional, costo y precio de venta mayor a cero.

#### Scenario: Creación exitosa de producto
- **WHEN** se envía un SKU no registrado previamente con nombre y precios positivos
- **THEN** el sistema guarda el producto en la base de datos con estado activo y retorna código HTTP 201 Created

#### Scenario: Rechazo por SKU duplicado
- **WHEN** se intenta registrar un producto cuyo código SKU ya existe en el sistema
- **THEN** el sistema rechaza la operación retornando código HTTP 409 Conflict con mensaje descriptivo

### Requirement: Consulta y búsqueda de productos
El sistema MUST permitir buscar productos por SKU, código de barras o coincidencia parcial de nombre.

#### Scenario: Búsqueda por SKU exacto
- **WHEN** se consulta `/api/v1/products/:sku` con un código existente
- **THEN** el sistema retorna la ficha completa del producto incluyendo su saldo de stock actual

#### Scenario: Producto inexistente
- **WHEN** se consulta un SKU que no se encuentra registrado
- **THEN** el sistema retorna código HTTP 404 Not Found
