---
description: >-
  Tablero de gobierno de TI para el CIO de Addi, construido sobre COBIT 2019 y el
  BSC de TI 2027–2030. Documentación técnica y de uso del Equipo Nexus.
---

# Dashboard estratégico de TI de Addi

El **Dashboard estratégico de TI** reúne en un solo lugar cómo está el gobierno de TI de Addi: la madurez de los 40 objetivos de COBIT 2019, los indicadores del Balanced Scorecard (BSC) de TI, el plan de acción, los riesgos y las decisiones que hay que tomar.

Además de mostrar datos, el tablero **propone cambios de nivel de madurez** cada mes y deja que la persona responsable de cada dominio COBIT los apruebe o rechace. Nada cambia sin una decisión humana.

{% hint style="warning" %}
**Datos de demostración.** Los diagnósticos COBIT y el BSC son reales, pero las mediciones mensuales son **simuladas** con reglas documentadas. Sirven para demostrar que el tablero funciona, no describen la situación real de Addi. Cada cifra lleva su etiqueta: *Real*, *Estimado* o *Simulado*.
{% endhint %}

## Por dónde empezar

| Quiero… | Ve a |
| --- | --- |
| Ver qué hace el tablero en 5 minutos | [Recorrido rápido](inicio/recorrido-rapido.md) |
| Entender cómo está construido | [Arquitectura](arquitectura/README.md) |
| Entender las reglas de COBIT y los semáforos | [Gobierno de TI](gobierno-ti/README.md) |
| Usar el tablero o aprobar un cambio | [Uso del tablero](uso/pestanas.md) |
| Correrlo en mi computador | [Desarrollo local](desarrollo-local/README.md) |
| Publicarlo en el servidor | [Despliegue](despliegue/README.md) |
| Resolver un error | [Solución de problemas](solucion-de-problemas.md) |

## En pocas palabras

* **Para quién:** el CIO/CTO de Addi y los líderes de cada dominio COBIT (EDM, APO, BAI, DSS, MEA).
* **Qué muestra:** madurez global (2,16 de 5 en el Diagnóstico de Control), 60 indicadores del BSC en cuatro perspectivas, 22 acciones del plan, 11 riesgos y el mapa de los 40 objetivos COBIT.
* **Qué hace solo:** cada corte mensual recalcula semáforos, genera alertas y propone subir o bajar el nivel de los objetivos COBIT.
* **Qué decide una persona:** aprobar o rechazar cada propuesta. El correo trae los botones; la decisión exige iniciar sesión.

## Tecnología

| Pieza | Tecnología |
| --- | --- |
| Datos y reglas | Python 3 (generador replicable) y PostgreSQL 16 con funciones SQL |
| API | FastAPI, SQLAlchemy 2 y Alembic |
| Interfaz | React 19, Vite, TypeScript, Tailwind 4 y ECharts |
| Automatización | n8n (corte mensual y correos) |
| Infraestructura | Docker Compose, Caddy (HTTPS), GitHub Actions, GHCR y AWS EC2 |

## Repositorios

* **Código:** [Julian270205/Dasboard-ADDI](https://github.com/Julian270205/Dasboard-ADDI)
* **Esta documentación:** se publica con GitBook a partir de su propio repositorio.

{% hint style="info" %}
Proyecto académico del **Equipo Nexus**. Los nombres de cargos (CTO, CISO, CPO…) se usan como roles de demostración.
{% endhint %}
