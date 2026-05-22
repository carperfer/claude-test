# Documentación de la API

Base URL: `http://localhost:8080/api/v1`

## Autenticación

### Registro

```
POST /auth/register
```

**Body:**
```json
{
  "nombre": "Juan García",
  "email": "juan@ejemplo.com",
  "password": "contraseña123"
}
```

**Respuesta 201:**
```json
{
  "message": "Usuario registrado correctamente",
  "user": { "id": 1, "nombre": "Juan García", "email": "juan@ejemplo.com" }
}
```

---

### Login

```
POST /auth/login
```

**Body:**
```json
{
  "email": "juan@ejemplo.com",
  "password": "contraseña123"
}
```

**Respuesta 200:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": { "id": 1, "nombre": "Juan García", "email": "juan@ejemplo.com" }
}
```

---

### Logout

```
POST /auth/logout
Authorization: Bearer <token>
```

**Respuesta 200:**
```json
{ "message": "Sesión cerrada" }
```

---

## Usuarios

### Obtener perfil propio

```
GET /users/me
Authorization: Bearer <token>
```

**Respuesta 200:**
```json
{
  "id": 1,
  "nombre": "Juan García",
  "email": "juan@ejemplo.com",
  "created_at": "2026-05-22T10:00:00Z"
}
```

---

### Actualizar perfil

```
PUT /users/me
Authorization: Bearer <token>
```

**Body:**
```json
{
  "nombre": "Juan García López"
}
```

---

## Códigos de error comunes

| Código | Significado |
|--------|-------------|
| 400 | Datos inválidos o faltantes |
| 401 | No autenticado / token inválido |
| 403 | Sin permisos |
| 404 | Recurso no encontrado |
| 422 | Error de validación |
| 500 | Error interno del servidor |
