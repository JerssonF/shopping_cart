## Diagrama de Secuencia — Flujo del Carrito de Compras

Descripción: diagrama de secuencia Mermaid que muestra los flujos principales: crear carrito, agregar producto y consultar el carrito/calcular totales.

```mermaid
sequenceDiagram
    participant Usuario
    participant Frontend
    participant Gateway
    participant Backend
    participant DB

    %% Crear carrito
    Note over Usuario,Frontend: Crear carrito
    Usuario->>Frontend: Solicita crear carrito
    Frontend->>Gateway: POST /api/carts
    Gateway->>Backend: POST /carts
    Backend->>DB: INSERT cart
    DB-->>Backend: {cartId} (201 Created)
    Backend-->>Gateway: 201 Created, {cartId}
    Gateway-->>Frontend: 201 Created, {cartId}
    Frontend-->>Usuario: Muestra carrito creado

    %% Agregar producto
    Note over Usuario,Frontend: Agregar producto al carrito
    Usuario->>Frontend: Agregar producto (cartId, sku, qty)
    Frontend->>Gateway: POST /api/carts/{cartId}/items
    Gateway->>Backend: POST /carts/{cartId}/items
    Backend->>DB: INSERT cart_item
    DB-->>Backend: OK (200)
    Backend->>DB: opcional UPDATE totals
    DB-->>Backend: OK
    Backend-->>Gateway: 200 OK, resumen de carrito
    Gateway-->>Frontend: 200 OK, resumen de carrito
    Frontend-->>Usuario: Muestra carrito actualizado

    %% Consultar carrito / calcular totales
    Note over Usuario,Frontend: Consultar carrito y calcular totales
    Usuario->>Frontend: Solicita ver carrito (cartId)
    Frontend->>Gateway: GET /api/carts/{cartId}
    Gateway->>Backend: GET /carts/{cartId}
    Backend->>DB: SELECT cart, items, precios
    DB-->>Backend: datos del carrito
    Backend->>Backend: Calcula totales (subtotal, impuestos, descuentos, total)
    Backend-->>Gateway: 200 OK, carrito con totales
    Gateway-->>Frontend: 200 OK, carrito con totales
    Frontend-->>Usuario: Muestra carrito con totales

    %% Opcional: eliminar producto
    Note over Usuario,Frontend: Eliminar producto
    Usuario->>Frontend: Eliminar item (cartId, itemId)
    Frontend->>Gateway: DELETE /api/carts/{cartId}/items/{itemId}
    Gateway->>Backend: DELETE /carts/{cartId}/items/{itemId}
    Backend->>DB: DELETE cart_item
    DB-->>Backend: OK
    Backend-->>Gateway: 200 OK, carrito actualizado
    Gateway-->>Frontend: 200 OK, carrito actualizado
    Frontend-->>Usuario: Muestra carrito actualizado

```

Notas:
- Los endpoints muestran el contrato público entre Frontend y Gateway; Gateway enruta al Backend.
- El cálculo de totales puede realizarse en el Backend leyendo precios actuales desde la base de datos y aplicando reglas de negocio (impuestos, descuentos, promociones).
