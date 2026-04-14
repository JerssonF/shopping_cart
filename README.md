# Shopping Cart

Repositorio de ejemplo que implementa un microservicio de carrito de compras. La solución está dividida en piezas independientes:

- `frontend`: Interfaz en Vue 3
- `gateway`: Punto de entrada (API Gateway) desarrollado con Spring Boot
- `backend`: Servicio principal en Spring Boot que contiene la lógica del carrito
- `PostgreSQL`: Almacenamiento relacional de datos
- `Docker Compose`: Orquesta los contenedores para entorno local

## Qué incluye

Las historias de usuario principales ya implementadas son:

- `HU-001` — Crear un carrito
- `HU-002` — Añadir un producto al carrito
- `HU-003` — Consultar el contenido del carrito

# Shopping Cart — Professional Overview

Repositorio diseñado como referencia y entorno de desarrollo para una solución de carrito de compras modular, con foco en buenas prácticas de despliegue y pruebas.

## Arquitectura

La aplicación está compuesta por servicios desacoplados:

- **Frontend**: SPA en Vue 3 (cliente que consume el API Gateway).
- **API Gateway**: Spring Boot encargado de exponer y orquestar rutas públicas.
- **Backend**: Microservicio Spring Boot con la lógica del carrito.
- **PostgreSQL**: Base de datos relacional para persistencia.
- **Docker Compose**: Orquestación para entorno local reproducible.

## Funcionalidades clave

- Creación y recuperación de carritos.
- Adición, actualización y eliminación de items.
- Cálculo de totales y resumen de carrito.
- Exposición de API mediante un gateway para desacoplamiento.

## Puertos por defecto (desarrollo)

- Frontend: `http://localhost:5180`
- Gateway: `http://localhost:8090`
- Backend: `http://localhost:8091`
- PostgreSQL: puerto `5030`

Si los puertos colisionan en tu entorno, edita `docker-compose.yml` y las variables `.env` correspondientes.

## Endpoints importantes

Todos los endpoints se exponen a través del `gateway`:

- `POST /api/v1/carts` — Crear carrito
- `POST /api/v1/carts/{cartId}/items` — Añadir item
- `PUT /api/v1/carts/{cartId}/items/{itemId}` — Actualizar item
- `DELETE /api/v1/carts/{cartId}/items/{itemId}` — Eliminar item
- `GET /api/v1/carts/{cartId}` — Obtener detalle del carrito
- `GET /api/v1/carts/{cartId}/total` — Obtener resumen del total

Ejemplo rápido — Crear carrito:

```bash
curl -s -X POST http://localhost:8090/api/v1/carts \
  -H "Content-Type: application/json" \
  -d '{"userId":1}'
```

Agregar producto:

```bash
curl -s -X POST http://localhost:8090/api/v1/carts/1/items \
  -H "Content-Type: application/json" \
  -d '{"productId":1,"name":"Mouse Logitech G203","quantity":2,"price":85000}'
```

Consultar carrito:

```bash
curl -s http://localhost:8090/api/v1/carts/1
```

Obtener total:

```bash
curl -s http://localhost:8090/api/v1/carts/1/total
```

## Levantar el entorno (rápido)

Requisitos: `docker` y `docker compose`.

Iniciar todos los servicios:

```bash
docker compose up --build
```

Detener y eliminar volúmenes (reset DB):

```bash
docker compose down -v
```

## Base de datos

La base por defecto es `shopping_cart_db`. El arranque del contenedor aplica `database/init.sql` y contempla migraciones con Liquibase bajo `database/`.

## Desarrollo local (frontend)

```bash
cd frontend
npm ci
npm run dev
```

La app de desarrollo quedará disponible en `http://localhost:5180`.

## Buenas prácticas

- El frontend debe consumir únicamente al `gateway` para mantener la capa de orquestación.
- Mantén las variables de entorno y `docker-compose.yml` en sincronía con los puertos que uses localmente.
- Para despliegues, externaliza las credenciales y usa secretos/variables del orquestador.

## Cómo contribuir

1. Fork o crea una rama `feature/...`.
2. Añade tests y documentación mínima para cambios relevantes.
3. Abre PR describiendo los cambios y cómo probarlos.

## Soporte

Abre un issue con el detalle y pasos para reproducir. Incluye logs y comandos ejecutados para acelerar la respuesta.

