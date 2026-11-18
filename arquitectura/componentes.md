---
description: Qué hace cada pieza del sistema, dónde está en el repositorio y cuánta memoria tiene.
---

# Componentes

## Contenedores en el servidor

| Contenedor | Imagen | Memoria | Expuesto | Función |
| --- | --- | --- | --- | --- |
| `web` | `ghcr.io/…/web` (Caddy + React compilado) | 64 MB | Puertos 80 y 443 | Sirve la interfaz, saca el certificado HTTPS y hace de proxy hacia la API |
| `api` | `ghcr.io/…/api` (Python 3.12) | 256 MB | No (solo red interna) | Login, roles, aprobaciones, reloj, auditoría |
| `postgres` | `postgres:16-alpine` | 256 MB | No | Bases `addi` y `n8n` |
| `n8n` | `n8nio/n8n` | 384 MB | Solo `127.0.0.1:5678` (por túnel SSH) | Cron mensual y envío de correos |

{% hint style="info" %}
La instancia tiene 1 GB de RAM. Por eso cada contenedor tiene un **límite de memoria** (`mem_limit`) y el servidor tiene **2 GB de swap**.
{% endhint %}

## Carpetas del repositorio de código

{% code title="Estructura" %}
```
datos/       Excel originales (evidencia): tres diagnósticos COBIT y la BD del BSC
generador/   extractores, catálogo, parametros.json, generar.py y huella.txt
db/          01_esquema · 02_datos_base · 03_mediciones · 04_funciones (los escribe el generador)
api/         FastAPI: app/ (auth, tablero, aprobaciones, admin, internal) · migrations/ · tests/
web/         React: src/pages (General, Estrategia, Temas, Cobit, Aprobaciones, Admin, Login)
n8n/         corte_y_avisos_cobit.json
infra/       docker-compose.yml · docker-compose.local.yml · Caddyfile · scripts/
.github/     workflows de CI y Deploy
```
{% endcode %}

## Detalle por pieza

<details>

<summary>Caddy (contenedor <code>web</code>)</summary>

* Obtiene y renueva solo el certificado de Let's Encrypt para el dominio de `SITE_ADDRESS`.
* Envía `/api/*` a la API y sirve la interfaz React para el resto de rutas.
* **Bloquea** `/api/v1/internal/*` (solo lo usa n8n por la red interna) y la documentación interactiva de la API.
* Agrega cabeceras de seguridad (HSTS, `X-Frame-Options`, `nosniff`).

</details>

<details>

<summary>API (contenedor <code>api</code>)</summary>

* Sesión con cookie `httpOnly` (JWT) y protección CSRF con doble cookie.
* Contraseñas con argon2 y límite de 5 intentos de login por minuto por IP.
* Roles: lector, aprobador y admin.
* Reloj idempotente: un mismo corte no se puede avanzar dos veces aunque lleguen dos peticiones a la vez.
* Toda decisión y acción de administración queda en la **auditoría**.

</details>

<details>

<summary>PostgreSQL (contenedor <code>postgres</code>)</summary>

* Un solo servidor con dos bases y dos usuarios: `addi` para la aplicación y `n8n` para n8n.
* No publica puertos. Solo es accesible desde la red interna de Docker.
* Respaldo diario a las 3:30 a. m. con 7 días de retención (ver [Respaldos](../operacion/respaldos.md)).

</details>

<details>

<summary>n8n</summary>

* Un solo flujo: `corte_y_avisos_cobit.json`.
* No tiene credenciales de la base. Habla con la API usando un token interno.
* Su editor no se publica en internet; se entra por túnel SSH.
* Guarda solo 7 días de ejecuciones para cuidar disco y memoria.

</details>
