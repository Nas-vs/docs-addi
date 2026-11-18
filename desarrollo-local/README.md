---
description: Cómo correr el proyecto completo en tu computador con Docker.
---

# Levantar el proyecto en tu PC

## Requisitos

| Necesitas | Para qué |
| --- | --- |
| **Docker** con Docker Compose | Correr todo el proyecto (obligatorio) |
| Git | Clonar el repositorio |
| Python 3.12 *(opcional)* | Trabajar en la API o el generador sin Docker |
| Node 22 *(opcional)* | Trabajar en la interfaz con recarga en caliente |

{% hint style="info" %}
En Windows, usa **WSL** (Ubuntu) y Docker Desktop con la integración de WSL activada. Clona el repositorio dentro de WSL, no en `C:\`.
{% endhint %}

## Paso a paso

{% stepper %}
{% step %}
### Clona el repositorio

```bash
git clone https://github.com/Julian270205/Dasboard-ADDI.git
cd Dasboard-ADDI
git checkout develop
```
{% endstep %}

{% step %}
### Crea el archivo .env local

```bash
cp .env.example infra/.env
for v in POSTGRES_SUPERPASSWORD APP_DB_PASSWORD N8N_DB_PASSWORD JWT_SECRET API_INTERNAL_TOKEN N8N_ENCRYPTION_KEY; do
  sed -i "s|^$v=.*|$v=$(openssl rand -hex 24)|" infra/.env
done
```

En local deja `ENV=local`, `SITE_ADDRESS=:80` y `PUBLIC_URL=http://localhost`.
{% endstep %}

{% step %}
### Levanta los contenedores

```bash
cd infra
alias dc='docker compose -p addi-dashboard -f docker-compose.yml -f docker-compose.local.yml'
dc up -d --build
```

En local las imágenes se construyen en tu PC. El archivo `docker-compose.local.yml` agrega **Mailpit**, un buzón de prueba para ver los correos.
{% endstep %}

{% step %}
### Carga los datos y crea los usuarios

```bash
dc exec api alembic upgrade head
dc exec api python -m app.cli cargar-datos
dc exec api python -m app.cli crear-admin --nombre "Tu nombre" --correo tu@correo.com
dc exec api python -m app.cli crear-usuarios-demo
```

Anota las contraseñas temporales que imprimen.
{% endstep %}

{% step %}
### Abre el tablero

| Servicio | Dirección |
| --- | --- |
| Tablero | http://localhost |
| n8n | http://localhost:5678 |
| Correos de prueba (Mailpit) | http://localhost:8025 |
| API directa | http://localhost:8000/api/v1/salud |
{% endstep %}
{% endstepper %}

## Configurar n8n en local

1. Entra a http://localhost:5678 y crea la cuenta de dueño.
2. Crea una credencial **SMTP** con host `mailpit`, puerto `1025`, sin usuario ni SSL.
3. Importa `n8n/corte_y_avisos_cobit.json`, asigna la credencial al nodo **Enviar correo al aprobador** y publícalo.

Los correos aparecen en http://localhost:8025.

## Trabajar en la interfaz con recarga en caliente

```bash
cd web
npm install
npm run dev
```

Abre http://localhost:5173. Las llamadas a `/api` van a la API del contenedor en el puerto 8000.

## Apagar y limpiar

{% tabs %}
{% tab title="Apagar (conserva datos)" %}
```bash
dc down
```
{% endtab %}

{% tab title="Borrar todo (datos incluidos)" %}
```bash
dc down -v
```

{% hint style="danger" %}
`-v` borra los volúmenes: la base de datos, n8n y sus credenciales. Después hay que volver a cargar datos y crear usuarios.
{% endhint %}
{% endtab %}
{% endtabs %}
