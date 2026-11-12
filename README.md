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

## Status

Work in progress. Docker Compose files, production configuration and
run instructions will be added here.

## Secrets

Never commit real passwords or keys. Local secrets go in a `.env` file,
which is ignored by git.
