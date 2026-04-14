# Shopping Cart

Implementación profesional y modular de un carrito de compras para arquitecturas basadas en microservicios.

Tabla de contenido
------------------
- Acerca de
- Características principales
- Arquitectura y estructura del repositorio
- Inicio rápido (Docker)
- Desarrollo local
- Ejecutar pruebas
- Contribuir
- Licencia

Acerca de
---------
Este repositorio contiene un proyecto completo de carrito de compras dividido en componentes de backend, frontend y gateway. Está pensado como una implementación de referencia para construir un servicio de carrito escalable con almacenamiento persistente, APIs REST y una interfaz moderna.

Características principales
--------------------------
- Crear y gestionar carritos asociados a usuarios autenticados
- Añadir, actualizar y eliminar artículos con cálculo de cantidades y subtotales
- Persistencia en base de datos con migraciones gestionadas por Liquibase
- SPA frontend que consume las APIs del carrito
- Configuración con Docker Compose para desarrollo local e integración

Arquitectura y estructura del repositorio
----------------------------------------
- `backend/` — Servicio backend en Java (Maven) que expone las APIs REST y la lógica del carrito
- `gateway/` — Servicio gateway (Maven) para enrutamiento e integración con autenticación
- `frontend/` — Aplicación SPA en Vue.js y código cliente
- `database/` — Changelogs de Liquibase y scripts SQL de migración
- `docker-compose.yml` — Archivo Compose para levantar toda la pila localmente

Inicio rápido (Docker)
----------------------
1. Construye y levanta los servicios con Docker Compose:

```bash
docker-compose up --build
```

2. Espera a que los servicios inicialicen (las migraciones se aplican automáticamente). Accede al frontend en `http://localhost:3000` (o el puerto configurado).

Desarrollo local
-----------------
Backend

```bash
cd backend
./mvnw spring-boot:run
```

Gateway

```bash
cd gateway
./mvnw spring-boot:run
```

Frontend

```bash
cd frontend
npm install
npm run dev
```

Base de datos

- Las migraciones están definidas en `database/` usando Liquibase. Para aplicar cambios manualmente utiliza la CLI de Liquibase configurada en el proyecto.

Ejecutar pruebas
----------------
- Pruebas unitarias/integración del backend: `./mvnw test` dentro de `backend` o `gateway`
- Pruebas del frontend: `npm test` dentro de `frontend`

Contribuir
----------
Las contribuciones son bienvenidas. Abre issues o pull requests con título y descripción claros. Sigue el estilo de código existente e incluye pruebas para nuevos comportamientos.

Licencia
--------
Este proyecto se entrega tal cual. Añade el fichero de licencia que prefieras en la raíz del repositorio.

Contacto
--------
Para preguntas o soporte, abre un issue en este repositorio.

