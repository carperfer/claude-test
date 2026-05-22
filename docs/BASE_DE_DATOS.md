# Base de datos

Motor: **MySQL 8.x**

## Tablas base

### `users`

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT UNSIGNED PK AI | Identificador único |
| nombre | VARCHAR(100) | Nombre completo |
| email | VARCHAR(150) UNIQUE | Correo electrónico |
| password | VARCHAR(255) | Hash bcrypt |
| email_verified_at | TIMESTAMP NULL | Fecha de verificación |
| created_at | DATETIME | Fecha de creación |
| updated_at | DATETIME | Última actualización |
| deleted_at | DATETIME NULL | Soft delete |

---

### `tokens_revocados`

Tabla para invalidar JWTs antes de su expiración (logout).

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT UNSIGNED PK AI | — |
| token_jti | VARCHAR(100) | JWT ID (claim `jti`) |
| expires_at | DATETIME | Expiración del token |
| created_at | DATETIME | Fecha de revocación |

---

## Migraciones

Las migraciones se gestionan con CodeIgniter Forge:

```bash
# Crear migración
docker compose exec php php spark make:migration CreateUsersTable

# Ejecutar migraciones pendientes
docker compose exec php php spark migrate

# Revertir última migración
docker compose exec php php spark migrate:rollback

# Ver estado
docker compose exec php php spark migrate:status
```

## Seeds

```bash
# Ejecutar todos los seeds
docker compose exec php php spark db:seed MainSeeder

# Seed específico
docker compose exec php php spark db:seed UsersSeeder
```
