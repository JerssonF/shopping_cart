# Diagrama UML de Casos de Uso

Este diagrama resume los casos de uso efectivamente soportados por el sistema según la implementación actual (frontend + gateway + backend).

## Diagrama

```mermaid
flowchart LR
    actor((Usuario))

    subgraph SC[Shopping Cart System]
        UC1((Crear carrito))
        UC2((Agregar producto))
        UC3((Consultar carrito))
        UC4((Actualizar cantidad))
        UC5((Eliminar producto))
        UC6((Consultar total))
    end

    actor --> UC1
    actor --> UC2
    actor --> UC3
    actor --> UC4
    actor --> UC5
    actor --> UC6

    UC2 -. incluye .-> UC3
    UC4 -. incluye .-> UC3
    UC5 -. incluye .-> UC3
    UC3 -. extiende .-> UC6
```

## Alcance

- Actor principal: `Usuario`.
- Flujo de operación vía frontend consumiendo API Gateway.
- Casos alineados con HU-001 a HU-007.
