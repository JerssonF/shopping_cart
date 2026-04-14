# Diagrama UML de Despliegue

Este diagrama muestra el despliegue actual del sistema en contenedores Docker y los puertos expuestos en el host.

## Diagrama

```mermaid
flowchart TB
    subgraph Host[Host local / Docker Engine]
        U[Usuario / Navegador]

        subgraph C1[Contenedor frontend]
            FE[Vue + Nginx]
            P1[5180:80]
        end

        subgraph C2[Contenedor gateway]
            GW[Spring Boot Gateway]
            P2[8090:8090]
        end

        subgraph C3[Contenedor backend]
            BE[Spring Boot Backend]
            P3[8091:8091]
        end

        subgraph C4[Contenedor postgres]
            DB[(PostgreSQL)]
            P4[5030:5432]
        end

        V[(Volumen: postgres_data)]
    end

    U -->|HTTP| FE
    FE -->|REST/JSON| GW
    GW -->|REST/JSON| BE
    BE -->|JDBC| DB
    DB --- V
```

## Puertos activos

- Frontend: `http://localhost:5180`
- Gateway: `http://localhost:8090`
- Backend: `http://localhost:8091`
- PostgreSQL: `localhost:5030`

## Notas

- El frontend consume únicamente al gateway.
- El gateway enruta operaciones hacia backend.
- El backend persiste datos en PostgreSQL.
- El volumen `postgres_data` conserva la información de la base entre reinicios.
