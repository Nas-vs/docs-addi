---
description: Cómo se conectan los datos, la API, la interfaz, n8n y el servidor.
---

# Vista general

El sistema tiene cuatro piezas que corren como contenedores de Docker en un solo servidor, más un generador de datos que corre aparte.

```
datos/*.xlsx ─► generador/ (Python) ─► db/01..04.sql ─► PostgreSQL · esquema addi
                                                              │   (datos + reglas en SQL)
                                                              │
                         API (FastAPI) ◄──────────────────────┘
                         login · roles · aprobaciones · reloj ─► esquema acceso
                           ▲           ▲
          Caddy (HTTPS) ───┘           └─── n8n: cron mensual y correos
          sirve la interfaz React           (sin acceso a la base)
```

## El recorrido de un dato

{% stepper %}
{% step %}
### Fuentes

Los tres diagnósticos COBIT y la base del BSC son archivos Excel en `datos/`. Se guardan como evidencia y no se editan.
{% endstep %}

{% step %}
### Generación

`generador/generar.py` lee esas fuentes y produce los scripts SQL `db/01` a `db/04`: el esquema, los datos base, las mediciones mensuales simuladas (con semilla fija) y las funciones SQL con las reglas del negocio.
{% endstep %}

{% step %}
### Base de datos

PostgreSQL guarda todo en el esquema `addi`. Ahí viven también las reglas: semáforos, alertas, propuestas COBIT y el reloj. La función `api_dashboard()` arma el JSON completo del tablero.
{% endstep %}

{% step %}
### API

La API en FastAPI es la **única que escribe** en la base. Controla el login, los roles, quién puede aprobar qué, el reloj y la auditoría. Guarda usuarios y sesiones en su propio esquema, `acceso`.
{% endstep %}

{% step %}
### Interfaz y automatización

La interfaz en React consume la API. n8n corre el corte mensual pidiéndoselo a la API y envía los correos que la API le arma.
{% endstep %}
{% endstepper %}

## Dos esquemas, una base

| Esquema | Lo escribe | Contiene |
| --- | --- | --- |
| `addi` | El generador (carga) y las funciones SQL | Indicadores, metas, mediciones, plan, riesgos, objetivos COBIT, alertas, propuestas, histórico de madurez |
| `acceso` | La API (migraciones de Alembic) | Usuarios, roles, aprobadores por dominio, sesiones, auditoría y ejecuciones del reloj |

n8n usa **otra base** (`n8n`) dentro del mismo servidor PostgreSQL, con su propio usuario.

## Sigue leyendo

* [Componentes](componentes.md): qué hace cada contenedor y cuánta memoria usa.
* [Decisiones de diseño](decisiones.md): por qué se construyó así.
