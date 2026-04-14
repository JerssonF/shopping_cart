# Diagrama UML de Contexto y Secuencia

Este documento muestra el contexto técnico real y el flujo de una operación típica en la arquitectura actual.

## 1. Diagrama de Componentes (UML)

```mermaid
flowchart LR
    U[Usuario]
    F[Frontend\nVue 3]
    G[Gateway\nSpring Boot]
    B[Backend\nSpring Boot]
    D[(PostgreSQL)]

    U -->|HTTP| F
    F -->|REST/JSON| G
    G -->|REST/JSON| B
    B -->|JPA/SQL| D
```

## 2. Diagrama de Secuencia (UML)

Escenario: agregar producto y consultar total actualizado.

```mermaid
sequenceDiagram
    actor Usuario
    participant Frontend
    participant Gateway
    participant Backend
    participant DB as PostgreSQL

    Usuario->>Frontend: Agregar producto al carrito
    Frontend->>Gateway: POST /api/v1/carts/{cartId}/items
    Gateway->>Backend: POST /api/v1/carts/{cartId}/items
    Backend->>DB: INSERT/UPDATE cart_items
    DB-->>Backend: OK
    Backend-->>Gateway: 201/200 + item
    Gateway-->>Frontend: 201/200 + item

    Frontend->>Gateway: GET /api/v1/carts/{cartId}/total
    Gateway->>Backend: GET /api/v1/carts/{cartId}/total
    Backend->>DB: SELECT carts + cart_items
    DB-->>Backend: datos
    Backend-->>Gateway: 200 + total
    Gateway-->>Frontend: 200 + total
    Frontend-->>Usuario: UI actualizada
```

## 3. Resumen

- El frontend no consume el backend directamente.
- El gateway centraliza acceso, validaciones de borde y enrutamiento.
- El backend concentra reglas de negocio del carrito.
- PostgreSQL persiste estado y permite reconstruir totales en cada consulta.
