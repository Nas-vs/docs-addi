---
description: Ramas, Pull Requests y cómo un cambio llega a producción.
---

# Flujo de trabajo con Git

## Ramas

| Rama | Qué es | Quién escribe |
| --- | --- | --- |
| `main` | Lo que está en producción | Solo por Pull Request desde `develop` |
| `develop` | Integración del equipo | Solo por Pull Request desde ramas de trabajo |
| `feat/…`, `fix/…`, `docs/…` | Trabajo de una persona sobre un tema | Su autor |

```
feat/mi-cambio ──PR──► develop ──PR──► main ──► despliegue automático
```

## Paso a paso

{% stepper %}
{% step %}
### Crea tu rama desde develop

```bash
git checkout develop
git pull
git checkout -b feat/mi-cambio
```
{% endstep %}

{% step %}
### Haz commits pequeños

Mensajes en presente, con el tipo y el área:

```bash
git commit -m "feat(api): permitir pausar el reloj"
git commit -m "fix(web): barra por perspectiva sin segmentos en cero"
```

| Tipo | Uso |
| --- | --- |
| `feat` | Funcionalidad nueva |
| `fix` | Corrección |
| `docs` | Documentación |
| `test` | Pruebas |
| `ci` | Workflows |
| `style` | Diseño o formato sin cambiar lógica |
{% endstep %}

{% step %}
### Sube la rama

```bash
git push -u origin feat/mi-cambio
```

Esto **solo** crea la rama en GitHub; no toca `develop` ni `main`.
{% endstep %}

{% step %}
### Abre el Pull Request

En GitHub: **base `develop`** ← compare `feat/mi-cambio`. Completa la plantilla y pide revisión.
{% endstep %}

{% step %}
### Espera la CI y la revisión

Los cuatro trabajos de la CI deben quedar en verde. Si algo falla, corrige en la misma rama y haz `push`; el PR se actualiza solo.
{% endstep %}

{% step %}
### Une con merge commit

Con la CI en verde y la aprobación, **Merge pull request → Create a merge commit**. Así se conservan los commits por tema.
{% endstep %}
{% endstepper %}

## Publicar en producción

Cuando el equipo aprueba, abre un PR de **`develop` → `main`**. Al unirlo, el workflow **Deploy** construye y despliega.

{% hint style="warning" %}
Nunca subas el `.env`, la llave `.pem` ni tokens. El `.gitignore` ya excluye `.env`, pero revisa `git status` antes de cada commit.
{% endhint %}

{% hint style="info" %}
Este flujo también es evidencia de gobierno: es **gestión de cambios (BAI06)** con trazabilidad de quién cambió qué y quién lo aprobó.
{% endhint %}

## Reescribir commits antes del merge

Si necesitas corregir mensajes o el autor de commits **que todavía no se han unido**, puedes reescribirlos y subirlos con:

```bash
git push --force-with-lease
```

`--force-with-lease` solo afecta a tu rama. Nunca reescribas `develop` ni `main`.
