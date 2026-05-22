# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Propósito

Repositorio base para MVPs: backend PHP/CodeIgniter, frontend React y base de datos MySQL en Docker. Sirve como punto de partida reutilizable con sistema de login/registro incluido y guías de despliegue para OVH Public Cloud y AWS EC2.

## Tech Stack

| Capa | Tecnología | Versión |
|---|---|---|
| Backend | PHP + CodeIgniter 4 | PHP 8.3 / CI 4.x |
| Frontend | React + Tailwind CSS | React 18 / Node 20 |
| Base de datos | MySQL / MariaDB | MySQL 8.x |
| Entorno local | Docker + Docker Compose | — |

## Comandos de desarrollo

**Levantar entorno local:**
```bash
docker compose up -d
```

**Parar entorno:**
```bash
docker compose down
```

**Ver logs:**
```bash
docker compose logs -f
```

**Frontend (dentro del contenedor o con Node local):**
```bash
npm install          # instalar dependencias
npm run dev          # servidor de desarrollo
npm run build        # build de producción (genera /dist o /build)
```

**Backend — migraciones:**
```bash
docker compose exec php php spark migrate
docker compose exec php php spark db:seed
```

## Estructura esperada del proyecto

```
/
├── backend/          # CodeIgniter 4
│   ├── app/
│   ├── public/
│   └── ...
├── frontend/         # React + Tailwind
│   ├── src/
│   └── ...
├── docker/           # Dockerfiles y configuración
├── docker-compose.yml
└── CLAUDE.md
```

## Despliegue

**Producción:**
- Backend PHP: despliegue vía GitHub Actions o script SSH manual al servidor OVH/AWS
- Frontend React: subir el build estático (`/dist`) al servidor o CDN
- No hay SSR — el frontend es 100% estático

**Entornos objetivo:**
- OVH Public Cloud (VPS/instancias)
- AWS EC2

## Convenciones

**PHP:** seguir PSR-12. Usar los modelos y controladores de CodeIgniter 4 sin frameworks adicionales.

**React/JS:** ESLint + Prettier. Componentes funcionales con hooks.

**Git:** mensajes de commit en español. Ramas en inglés (`feature/`, `fix/`, `hotfix/`).

## Planificación

- Ante tareas complejas o ambiguas, pedir aclaración antes de codificar
- Preferir estructuras simples y fáciles de mantener sobre soluciones sofisticadas
- El sistema de autenticación base (login/registro) es el núcleo — no romperlo al añadir funcionalidades
