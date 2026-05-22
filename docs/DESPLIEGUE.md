# Guía de despliegue

## Entornos objetivo

| Entorno | Plataforma | Método |
|---------|-----------|--------|
| Producción | OVH Public Cloud / AWS EC2 | GitHub Actions + SSH |
| Manual | Cualquier VPS | Script bash |

---

## Despliegue automático con GitHub Actions

El pipeline se activa en cada push a `main`.

### Variables de entorno requeridas en GitHub Secrets

```
SSH_HOST         → IP o dominio del servidor
SSH_USER         → Usuario SSH (ej: ubuntu, debian)
SSH_KEY          → Clave privada SSH (contenido completo)
SSH_PORT         → Puerto SSH (por defecto 22)
DEPLOY_PATH      → Ruta absoluta en el servidor (ej: /var/www/html)
```

### Flujo del pipeline

1. Checkout del código
2. Build del frontend React (`npm run build`)
3. Subida del backend PHP por SSH/rsync
4. Subida del build estático del frontend
5. Ejecutar migraciones en remoto
6. Reload de Apache/Nginx

---

## Despliegue manual

```bash
# Backend — sincronizar archivos PHP (excluye node_modules, .git, docker)
rsync -avz --exclude='.git' --exclude='frontend/node_modules' \
  backend/ usuario@servidor:/var/www/html/backend/

# Frontend — subir build estático
cd frontend && npm run build
rsync -avz dist/ usuario@servidor:/var/www/html/public/

# Migraciones en remoto
ssh usuario@servidor "cd /var/www/html/backend && php spark migrate"
```

---

## Configuración del servidor

### Requisitos mínimos

- PHP 8.3 + extensiones: `mbstring`, `intl`, `curl`, `pdo_mysql`
- MySQL 8.x
- Apache 2.4 o Nginx
- Certbot (SSL)

### Virtual host Apache (ejemplo)

```apache
<VirtualHost *:80>
    ServerName midominio.com
    DocumentRoot /var/www/html/backend/public

    <Directory /var/www/html/backend/public>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

El frontend estático puede servirse desde el mismo Apache en un subdirectorio o desde un dominio/subdominio distinto.

---

## Notas importantes

- El archivo `.env` de producción **nunca** se sube al repositorio
- Las variables de entorno se configuran manualmente en el servidor la primera vez
- Hacer backup de la BD antes de ejecutar migraciones en producción
