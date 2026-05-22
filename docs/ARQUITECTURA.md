# Arquitectura del proyecto

## Visión general

Arquitectura desacoplada: backend API REST en PHP/CodeIgniter 4 y frontend SPA en React. Se comunican exclusivamente a través de la API.

```
┌─────────────────┐         ┌─────────────────────┐
│   React (SPA)   │  HTTP   │  CodeIgniter 4 (API) │
│   Tailwind CSS  │ ──────► │  PHP 8.3             │
│   Port: 5173    │         │  Port: 8080           │
└─────────────────┘         └──────────┬──────────┘
                                        │
                                        ▼
                             ┌─────────────────────┐
                             │   MySQL 8.x          │
                             │   Port: 3306         │
                             └─────────────────────┘
```

## Estructura de carpetas

```
/
├── backend/                  # CodeIgniter 4
│   ├── app/
│   │   ├── Controllers/      # Controladores de la API
│   │   ├── Models/           # Modelos de base de datos
│   │   ├── Filters/          # Middlewares (auth JWT, CORS)
│   │   └── Database/
│   │       ├── Migrations/   # Migraciones de BD
│   │       └── Seeds/        # Datos de prueba
│   └── public/               # Punto de entrada (index.php)
│
├── frontend/                 # React 18
│   ├── src/
│   │   ├── components/       # Componentes reutilizables
│   │   ├── pages/            # Vistas/páginas
│   │   ├── hooks/            # Custom hooks
│   │   ├── services/         # Llamadas a la API
│   │   └── context/          # Estado global (AuthContext, etc.)
│   └── dist/                 # Build de producción (no commitear)
│
├── docker/                   # Configuración Docker
│   ├── php/
│   └── mysql/
├── docker-compose.yml
└── docs/
```

## Flujo de autenticación

1. Usuario hace login → frontend llama a `POST /api/auth/login`
2. Backend valida credenciales → devuelve JWT
3. Frontend almacena el JWT (localStorage o cookie httpOnly)
4. Todas las peticiones protegidas incluyen `Authorization: Bearer <token>`
5. El Filter de CI4 valida el token antes de ejecutar el controlador

## Convenciones de la API

- Base URL: `/api/v1/`
- Formato de respuesta: JSON siempre
- Autenticación: JWT (Bearer token)
- Errores: código HTTP apropiado + `{ "error": "mensaje" }`
