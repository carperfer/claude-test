# Configuración del entorno local

## Requisitos previos

- Docker Desktop instalado y en ejecución
- Node.js 20.x
- Git

## Pasos iniciales

### 1. Clonar el repositorio

```bash
git clone https://github.com/carperfer/claude-test.git
cd claude-test
```

### 2. Levantar el entorno Docker

```bash
docker compose up -d
```

Esto levanta:
- **PHP 8.3** con Apache en `http://localhost:8080`
- **MySQL 8.x** en el puerto `3306`
- **phpMyAdmin** (opcional) en `http://localhost:8081`

### 3. Instalar dependencias del frontend

```bash
cd frontend
npm install
npm run dev
```

El frontend estará disponible en `http://localhost:5173`

### 4. Ejecutar migraciones

```bash
docker compose exec php php spark migrate
docker compose exec php php spark db:seed
```

## Variables de entorno

Copia `.env.example` a `.env` y configura:

```
CI_ENVIRONMENT=development
database.default.hostname=mysql
database.default.database=app_db
database.default.username=root
database.default.password=secret
```

## Verificar que todo funciona

- Backend: `http://localhost:8080/api/health`
- Frontend: `http://localhost:5173`
