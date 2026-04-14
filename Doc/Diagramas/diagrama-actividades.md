# Diagrama UML de Actividades

Este diagrama describe el flujo completo de gestión del carrito desde la UI hasta persistencia y respuesta.

## Diagrama

```mermaid
stateDiagram-v2
    [*] --> AbrirFrontend
    AbrirFrontend --> RevisarCarrito: verificar cartId local

    state RevisarCarrito <<choice>>
    RevisarCarrito --> CrearCarrito: No existe cartId
    RevisarCarrito --> ConsultarCarrito: Existe cartId

    CrearCarrito --> ProcesarSolicitud: POST /api/v1/carts
    ConsultarCarrito --> ProcesarSolicitud: GET /api/v1/carts/:cartId

    ProcesarSolicitud --> ValidarNegocio
    ValidarNegocio --> PersistenciaInicial
    PersistenciaInicial --> RespuestaInicial
    RespuestaInicial --> ElegirAccion

    state ElegirAccion <<choice>>
    ElegirAccion --> AgregarProducto: agregar producto
    ElegirAccion --> ActualizarCantidad: actualizar cantidad
    ElegirAccion --> EliminarProducto: eliminar producto
    ElegirAccion --> ConsultarTotal: consultar total

    AgregarProducto --> ProcesarOperacion: POST /api/v1/carts/:cartId/items
    ActualizarCantidad --> ProcesarOperacion: PUT /api/v1/carts/:cartId/items/:itemId
    EliminarProducto --> ProcesarOperacion: DELETE /api/v1/carts/:cartId/items/:itemId
    ConsultarTotal --> ProcesarOperacion: GET /api/v1/carts/:cartId/total

    ProcesarOperacion --> AplicarReglas
    AplicarReglas --> PersistenciaOperacion
    PersistenciaOperacion --> RespuestaOperacion
    RespuestaOperacion --> ActualizarPantalla

    ActualizarPantalla --> Continuar
    state Continuar <<choice>>
    Continuar --> ElegirAccion: Sí
    Continuar --> [*]: No
```

## Reglas clave del flujo

- Si el carrito no existe, primero se crea.
- Todas las operaciones pasan por gateway.
- El backend valida, ejecuta reglas de negocio y persiste cambios.
- El frontend refresca carrito y total después de cada operación.
