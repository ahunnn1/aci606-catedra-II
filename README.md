# ACI606 - Docker Rescue Challenge

Aplicación web multi-servicio recuperada y ejecutable con Docker Compose.

## Arquitectura

Frontend (Nginx, :80) -> Backend (FastAPI, :8000) -> Base de datos (PostgreSQL 16)

- Solo el frontend se publica al host: `http://localhost:8080`.
- El frontend reenvía `/api/*` al servicio `backend`; el backend se conecta a `db`.
- Los datos de PostgreSQL persisten en el volumen nombrado `pgdata`.
- El servicio `db` tiene un healthcheck (`pg_isready`) y el backend espera a que esté sano.

## Requisitos

Docker y Docker Compose v2.

## Configuración

mv .env.ej .env

Variables (con valores por defecto si no existe `.env`): `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`, `FRONTEND_PORT`.

## Iniciar

docker compose up --build -d

## Verificar

docker compose ps
curl -s http://localhost:8080/api/health
curl -s -X POST http://localhost:8080/api/items -H "Content-Type: application/json" -d '{"name":"prueba"}'
curl -s http://localhost:8080/api/items

Respuesta esperada de salud: {"status":"ok","database":"reachable"}

## Logs y diagnóstico

docker compose logs <frontend|backend|db>
docker compose config
docker inspect <contenedor>

## Detener

docker compose down        # conserva los datos
docker compose down -v     # elimina también el volumen (borra los datos)

## Recrear

docker compose down
docker compose up --build -d

Los datos persisten tras `down` + `up` gracias al volumen `pgdata`.

## Reparaciones

El detalle de cada incidencia (R-01 a R-07) está en el informe técnico y en el historial de commits.
