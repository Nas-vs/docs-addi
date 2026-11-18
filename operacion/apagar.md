---
description: Qué borrar al terminar el proyecto para no seguir pagando ni dejar accesos abiertos.
---

# Apagar todo al terminar

Cuando el proyecto termine, sigue esta lista para que no se generen cobros ni queden accesos abiertos.

{% hint style="warning" %}
Antes de borrar el servidor, **descarga un respaldo** si quieren conservar los datos (ver [Respaldos](respaldos.md)).
{% endhint %}

## AWS

- [ ] **EC2 → Instances →** selecciona la instancia → **Instance state → Terminate**.
- [ ] **EC2 → Elastic IPs →** selecciona la IP → **Actions → Release**. Una IP sin usar sigue cobrando.
- [ ] **EC2 → Volumes:** verifica que no queden discos sueltos.
- [ ] **EC2 → Snapshots:** borra los que hayan quedado.
- [ ] **Key pairs:** borra el par de llaves si no lo van a usar más.

## GitHub

- [ ] Borra los secretos `EC2_HOST`, `EC2_USER`, `EC2_SSH_KEY` y `GHCR_PAT`.
- [ ] Revoca el token `GHCR_PAT` en **Settings → Developer settings → Personal access tokens**.
- [ ] Opcional: desactiva el workflow **Deploy** para que un merge no intente desplegar.

## Otros servicios

- [ ] **DuckDNS:** borra el subdominio o déjalo apuntando a nada.
- [ ] **Brevo:** borra la SMTP key en **SMTP & API**.

## Lo que se conserva

El repositorio de código, esta documentación y las imágenes en GHCR siguen disponibles. Con ellos se puede volver a desplegar en otro servidor siguiendo la sección [Despliegue](../despliegue/README.md).
