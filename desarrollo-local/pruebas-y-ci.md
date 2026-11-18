---
description: Cómo correr las pruebas y qué revisa la integración continua en cada Pull Request.
---

# Pruebas y CI

## Correr las pruebas en tu PC

{% tabs %}
{% tab title="API" %}
```bash
cd api
pip install -r requirements-dev.txt
ruff check .
pytest -q
```

Sin la variable `DATABASE_URL`, `pytest` corre solo las pruebas que no necesitan base de datos (por ejemplo, las reglas de permisos).
{% endtab %}

{% tab title="Interfaz" %}
```bash
cd web
npm install
npm run build
```

`npm run build` revisa los tipos de TypeScript y compila. Si hay un error de tipos, falla.
{% endtab %}

{% tab title="Generador" %}
```bash
cd generador
python3 generar.py
git diff --exit-code ../db/ huella.txt
```

Si `git diff` muestra cambios, los datos de `db/` no corresponden al generador.
{% endtab %}
{% endtabs %}

## Qué revisa la CI

El workflow **CI** corre en cada Pull Request y en cada push a `develop` y `main`. Tiene cuatro trabajos:

| Trabajo | Qué verifica |
| --- | --- |
| **Generador** | Que el generador sea replicable y que `db/` y `huella.txt` estén al día |
| **api** | Lint con `ruff` y pruebas con `pytest` contra un PostgreSQL con los datos de `db/` cargados |
| **web** | Que la interfaz compile y no tenga errores de tipos |
| **n8n** | Que los flujos sean JSON válido y no traigan credenciales |

{% hint style="success" %}
Un Pull Request solo se une cuando **los cuatro trabajos están en verde** y tiene al menos una revisión aprobada.
{% endhint %}

## Qué prueban las pruebas de la API

* Que el tablero exija sesión.
* Que el tablero devuelva el JSON completo con los datos cargados.
* Que las acciones que cambian algo exijan el token CSRF.
* Que un **lector** y un **admin** no puedan aprobar.
* Que rechazar exija motivo y que aprobar cree una versión nueva del nivel.
* Que el reloj sea **idempotente**: avanzar dos veces el mismo corte no lo mueve dos meses.
* Que los endpoints internos (los que usa n8n) exijan el token interno.
* Las reglas de permiso: decide el titular, el suplente espera el escalado, un lector no decide aunque sea titular y una propuesta decidida no se vuelve a decidir.
