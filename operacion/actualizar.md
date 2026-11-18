---
description: Cómo llega un cambio del código a producción y cómo volver atrás.
---

# Actualizar la aplicación

## El camino normal

Cualquier cambio sigue el mismo flujo. No se toca el servidor a mano.

{% stepper %}
{% step %}
### Rama de trabajo

Desde `develop`, crea una rama `feat/…`, `fix/…` o `docs/…` y haz tus commits.
{% endstep %}

{% step %}
### Pull Request a develop

La CI debe pasar y al menos una persona revisa. Se une con *merge commit*.
{% endstep %}

{% step %}
### Pull Request de develop a main

Cuando el equipo aprueba publicar. Al unirlo, el workflow **Deploy** construye las imágenes y las despliega solo.
{% endstep %}
{% endstepper %}

Ver más en [Flujo de trabajo con Git](../referencia/flujo-git.md).

## Qué hace el servidor en cada despliegue

{% code title="infra/scripts/deploy.sh (resumen)" %}
```bash
docker compose pull                                     # descarga las imágenes nuevas
docker compose up -d --no-build --remove-orphans        # las arranca
docker compose exec -T api alembic upgrade head         # migraciones del esquema acceso
docker compose exec -T api python -m app.cli cargar-datos --si-cambio   # datos, solo si cambió la versión
docker image prune -f                                   # limpia imágenes viejas
```
{% endcode %}

{% hint style="info" %}
`cargar-datos --si-cambio` solo recarga el esquema `addi` si la versión de parámetros del generador es distinta a la cargada. Si no cambió, los datos, las decisiones y el reloj se conservan.
{% endhint %}

## Volver a una versión anterior

Cada imagen se publica con dos etiquetas: `latest` y el **hash del commit**.

{% tabs %}
{% tab title="Revertir en Git (recomendado)" %}
Revierte el commit problemático en `develop`, súbelo por PR y luego a `main`. El despliegue automático deja el servidor como estaba.

```bash
git revert <hash-del-commit>
```
{% endtab %}

{% tab title="Fijar una versión en el servidor (emergencia)" %}
```bash
cd /opt/addi-dashboard
TAG=<hash-del-commit-anterior> docker compose up -d --no-build
```

Es temporal: el siguiente despliegue vuelve a `latest`.
{% endtab %}
{% endtabs %}

## Cambiar un valor de configuración

Edita la línea en el `.env` y reinicia solo lo necesario:

```bash
cd /opt/addi-dashboard
sed -i "s|^CORREO_REMITENTE=.*|CORREO_REMITENTE=nuevo@correo.com|" .env
docker compose up -d n8n api
```

{% hint style="danger" %}
No cambies las contraseñas de PostgreSQL en el `.env` sin cambiarlas también dentro de la base: la API y n8n dejarían de conectarse.
{% endhint %}
