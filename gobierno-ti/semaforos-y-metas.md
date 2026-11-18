---
description: Cómo calcula el tablero el color de cada indicador y la meta de cada mes.
---

# Semáforos y metas

## La meta "a la fecha"

Las metas del BSC son anuales (2027, 2028, 2029 y 2030). Comparar un valor de marzo con la meta de diciembre sería injusto, así que el tablero calcula una **meta a la fecha del corte**: interpola entre la meta del año anterior y la del año en curso según el mes.

{% code title="Ejemplo" %}
```
Meta 2027 = 60 %   Meta 2028 = 80 %   Corte: junio de 2028
Meta a la fecha = 60 + (80 − 60) × 6 / 12 = 70 %
```
{% endcode %}

Hasta 2026 se usa la línea base si existe; si no, la meta de 2027.

## Cómo se decide el color

Cada indicador tiene un **sentido**:

* **Max**: más es mejor (por ejemplo, disponibilidad).
* **Min**: menos es mejor (por ejemplo, cambios fallidos).
* **Rango**: lo sano es estar entre un mínimo y un máximo.

{% tabs %}
{% tab title="Regla general del BSC" %}
Se calcula el **cumplimiento** frente a la meta a la fecha:

| Cumplimiento | Color |
| --- | --- |
| 100 % o más | 🟢 Verde |
| 80 % a 99 % | 🟡 Amarillo |
| Menos de 80 % | 🔴 Rojo |

Para indicadores *Max*: cumplimiento = valor ÷ meta. Para *Min*: cumplimiento = meta ÷ valor.
{% endtab %}

{% tab title="Con umbral de alerta" %}
Algunos indicadores tienen un **umbral rojo** absoluto definido en el catálogo:

| Sentido | Verde | Amarillo | Rojo |
| --- | --- | --- | --- |
| Max | valor ≥ meta | entre umbral y meta | valor < umbral |
| Min | valor ≤ meta | entre meta y umbral | valor > umbral |
{% endtab %}

{% tab title="Indicadores de rango" %}
| Situación | Color |
| --- | --- |
| Dentro del rango sano | 🟢 Verde |
| Hasta 20 % fuera del rango | 🟡 Amarillo |
| Más lejos | 🔴 Rojo |
{% endtab %}
{% endtabs %}

Un indicador sin medición o sin meta aparece en **gris**.

{% hint style="info" %}
En el tablero el color nunca va solo: cada estado tiene también una **forma** (● verde, ▲ amarillo, ■ rojo, ○ sin dato), para que se entienda sin depender del color.
{% endhint %}

## Alertas

Cada vez que un indicador queda en **rojo** en un corte, se crea una alerta con:

* **Qué pasa:** el valor frente a la meta o el umbral.
* **Impacto** y **qué decidir**, tomados del catálogo del indicador.
* **Responsable** del indicador.
* **Fecha límite:** corte + 1 mes + 14 días.

Las alertas se ven en la pestaña General, en *Qué requiere una decisión*.

## Dos relojes

| Reloj | Corte inicial | Mueve |
| --- | --- | --- |
| **Operativo** | Noviembre de 2026 | Indicadores de operación, seguridad, datos e IA |
| **Del plan** | Junio de 2028 | Avance y gasto del plan de acción y la madurez proyectada |

**Avanzar un mes** mueve los dos relojes a la vez. El horizonte termina en diciembre de 2030.
