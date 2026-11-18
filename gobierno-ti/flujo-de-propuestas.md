---
description: >-
  Cómo propone el tablero cambios de nivel COBIT cada corte, quién los aprueba y
  qué pasa si nadie responde.
---

# Flujo de propuestas COBIT

{% hint style="success" %}
**Regla de diseño:** el flujo propone y una persona aprueba. Un nivel de capacidad COBIT es un juicio con evidencia; si se moviera solo, no se podría defender.
{% endhint %}

## Qué pasa en cada corte

{% stepper %}
{% step %}
### Avanza el reloj

El cron de n8n (día 1 de cada mes, 7:00 a. m.) o el botón **Avanzar un mes** le piden a la API que avance el corte. La API valida que existan mediciones para el mes nuevo; si faltan, el reloj no se mueve.
{% endstep %}

{% step %}
### Se recalcula el tablero

Semáforos con la meta a la fecha, alertas nuevas para los indicadores en rojo y un recordatorio más para cada propuesta pendiente.
{% endstep %}

{% step %}
### Se aplican las reglas de propuesta

Ver la tabla de reglas más abajo. Nunca se propone un salto de más de un nivel.
{% endstep %}

{% step %}
### Se envían los correos

La API arma **un correo por dominio** con propuestas o alertas en ese corte, y n8n lo envía. Cada propuesta trae nivel actual, nivel propuesto, regla, evidencia, etiqueta y los botones **Aprobar** y **Rechazar**.
{% endstep %}

{% step %}
### Una persona decide

El botón abre el tablero, que pide iniciar sesión. Solo el titular del dominio (o el suplente, si la propuesta escaló) puede decidir.
{% endstep %}

{% step %}
### Se crea una versión nueva

Aprobar crea una fila nueva en el histórico de madurez con fecha, aprobador y evidencia. La versión anterior no se borra ni se edita. Rechazar exige escribir el motivo.
{% endstep %}
{% endstepper %}

## Reglas de propuesta

| Regla | Condición | Resultado |
| --- | --- | --- |
| **Subir nivel** | Todas las acciones del plan ligadas al objetivo están completadas y sus indicadores llevan 2 cortes seguidos en verde | Propone subir 1 nivel |
| **Bajar nivel** | Un indicador ligado lleva 2 cortes seguidos en rojo | Propone bajar 1 nivel |
| **Alerta de riesgo** | Riesgo crítico (probabilidad × impacto ≥ 6) ligado al objetivo sin acción asociada | Alerta, sin cambio de nivel |
| **Alerta de brecha** | Importancia 5 con desempeño bajo, o proceso en nivel 1 | Alerta, sin cambio de nivel |
| **Alerta de plan** | Acción del plan ligada al objetivo y atrasada (avance frente a gasto menor a 0,9) | Alerta, sin cambio de nivel |

## Quién aprueba

| Dominio | Titular (demo) | Suplente (demo) |
| --- | --- | --- |
| EDM | CTO | Oficial de Cumplimiento |
| APO | Head of Technology Risk | CTO |
| BAI | CPO | CTO |
| DSS | Líder de Infraestructura | CTO |
| MEA | Oficial de Cumplimiento | CTO |

Las personas se cambian en **Administración → Aprobadores**.

## Casos especiales

<details>

<summary>El titular no responde</summary>

Cada corte sin decisión suma un recordatorio. Cuando una propuesta llega a **2 cortes sin respuesta** (`CORTES_PARA_ESCALAR`), el suplente también recibe el correo y puede decidir. Mientras tanto el cambio sigue pendiente y **no se aplica**.

</details>

<details>

<summary>El aprobador es dueño de las acciones que justifican el cambio</summary>

El correo se envía **con copia al CTO** como segunda revisión, para que nadie apruebe su propia nota sin que otro lo vea.

</details>

<details>

<summary>El nivel cambió desde que se propuso</summary>

Si entre la propuesta y la decisión el nivel vigente ya cambió, la propuesta **caduca** y no se aplica. Así no se aprueba sobre una base vieja.

</details>

<details>

<summary>Un administrador intenta aprobar</summary>

Ser administrador no da permiso para aprobar. Solo decide quien sea titular o suplente del dominio y tenga el rol aprobador.

</details>

{% hint style="info" %}
En la demo el corte es mensual. En producción sería trimestral para que los niveles no suban más rápido que la realidad.
{% endhint %}
