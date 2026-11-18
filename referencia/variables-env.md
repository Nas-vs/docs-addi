---
description: Todas las variables del archivo .env, qué hacen y qué valor llevan.
---

# Variables del .env

El `.env` vive en `infra/.env` en local y en `/opt/addi-dashboard/.env` en el servidor. **Nunca se sube al repositorio.**

## Imágenes

| Variable | Producción | Local | Para qué |
| --- | --- | --- | --- |
| `REGISTRY` | `ghcr.io/julian270205/dasboard-addi` | igual | Dónde están las imágenes. **En minúsculas** |
| `TAG` | `latest` | `latest` | Versión de las imágenes. El deploy la fija al hash del commit |

## Dominio

| Variable | Producción | Local | Para qué |
| --- | --- | --- | --- |
| `SITE_ADDRESS` | `addi-ti.duckdns.org` | `:80` | Dominio para el que Caddy saca el certificado HTTPS |
| `PUBLIC_URL` | `https://addi-ti.duckdns.org` | `http://localhost` | Dirección que usan los botones de los correos |

## PostgreSQL

| Variable | Valor | Para qué |
| --- | --- | --- |
| `POSTGRES_SUPERUSER` | `postgres` | Superusuario del servidor de base de datos |
| `POSTGRES_SUPERPASSWORD` | 🔒 aleatoria | Su contraseña |
| `APP_DB` / `APP_DB_USER` / `APP_DB_PASSWORD` | `addi` / `addi_api` / 🔒 | Base y usuario de la aplicación |
| `N8N_DB` / `N8N_DB_USER` / `N8N_DB_PASSWORD` | `n8n` / `n8n` / 🔒 | Base y usuario de n8n |

## API

| Variable | Valor | Para qué |
| --- | --- | --- |
| `ENV` | `produccion` (servidor) · `local` | En producción las cookies solo viajan por HTTPS y se rechazan secretos de desarrollo |
| `JWT_SECRET` | 🔒 aleatoria | Firma las sesiones |
| `SESSION_HOURS` | `8` | Duración de la sesión |
| `API_INTERNAL_TOKEN` | 🔒 aleatoria | Token compartido entre la API y n8n (cabecera `X-Internal-Token`) |
| `CORTES_PARA_ESCALAR` | `2` | Cortes sin respuesta antes de que pueda decidir el suplente |
| `CORS_ORIGINS` | vacío | Solo si la interfaz corre en otro origen durante el desarrollo |

## n8n y correo

| Variable | Valor | Para qué |
| --- | --- | --- |
| `N8N_VERSION` | `latest` | Versión de la imagen de n8n |
| `N8N_ENCRYPTION_KEY` | 🔒 aleatoria | Cifra las credenciales guardadas en n8n. **Si cambia, n8n no puede leerlas** |
| `CORREO_REMITENTE` | Remitente verificado en Brevo | Dirección "De" de los correos |

{% hint style="warning" %}
Las variables marcadas con 🔒 se generan una sola vez. Cambiarlas después rompe la conexión con datos que ya existen (la base o las credenciales de n8n).
{% endhint %}

{% hint style="info" %}
Las credenciales de Brevo (Login y SMTP key) **no** van en el `.env`: se guardan cifradas dentro de n8n.
{% endhint %}
