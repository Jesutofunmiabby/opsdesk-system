# OpsDesk System

Deployment and documentation repo for **OpsDesk**, an IT help-desk ticketing app.

OpsDesk is split into three repos:

| Repo | What it holds |
| --- | --- |
| [opsdesk-frontend](https://github.com/Jesutofunmiabby/opsdesk-frontend) | React + Vite + TypeScript web app |
| [opsdesk-backend](https://github.com/Jesutofunmiabby/opsdesk-backend) | Spring Boot (Java 25) REST API with PostgreSQL |
| **opsdesk-system** (this repo) | Docker Compose setup, production settings and project docs |

## Local layout

The three repos are expected to sit side by side:

```
opsdesk/
├── opsdesk-backend/
├── opsdesk-frontend/
└── opsdesk-system/
```

## Run everything with Docker

You need [Docker Desktop](https://www.docker.com/products/docker-desktop/)
running, and the three repositories cloned side by side as shown above.

1. In this folder, copy the example settings and fill in real values:

   ```bash
   cp .env.example .env
   ```

   Replace `DB_PASSWORD` and `JWT_SECRET` with your own values. Make a
   secret with `openssl rand -base64 48`.

2. Build and start the database, the backend and the frontend:

   ```bash
   docker compose up --build
   ```

3. Open <http://localhost:3000>, register an account, log in and create a
   ticket.

Stop everything with Ctrl+C, or with `docker compose down` from another
terminal. Your data is kept in the `dbdata` volume, so it is still there
after `docker compose down` and `docker compose up`. Only
`docker compose down -v` deletes it.

| Service | What it is | Address |
| --- | --- | --- |
| `db` | PostgreSQL 16. Only the backend can reach it. | `db:5432` inside Docker |
| `backend` | The Spring Boot API, with the `prod` profile | <http://localhost:8080> |
| `frontend` | The React app, served by nginx | <http://localhost:3000> |

The backend waits until the database's health check says it accepts
connections. You can check the backend is up with
<http://localhost:8080/actuator/health>.

### Make the first admin

Every new account is a `USER`. To make your account an `ADMIN`, run:

```bash
docker compose exec db psql -U opsdesk -d opsdesk -c "update app_user set role = 'ADMIN' where email = 'you@example.com'"
```

It prints `UPDATE 1`. Log out and log in again: the role is stored in the
login token, so the old token still says `USER`.

## Secrets

Never commit real passwords or keys. Local secrets go in a `.env` file,
which is ignored by git. [`.env.example`](.env.example) lists every setting.

PostgreSQL sets its user and password only when the `dbdata` volume is first
created. If you change `DB_USER` or `DB_PASSWORD` later, either change them
in the database too, or delete the data with `docker compose down -v`.
