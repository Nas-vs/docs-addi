---
description: Un recorrido de 5 minutos por lo que hace el tablero, ideal para una demo.
---

# Recorrido rápido

Este recorrido sirve como guion para una presentación. Necesitas un usuario **administrador** y uno **aprobador** (ver [Desplegar y crear usuarios](../despliegue/desplegar.md)).

{% stepper %}
{% step %}
### Resumen ejecutivo (pestaña General)

Muestra la **madurez global de 2,16** (promedio del Diagnóstico de Control, escala 0 a 5), el radar por dominio COBIT, los semáforos por perspectiva del BSC y las **alertas que requieren decisión**, cada una con responsable y fecha límite.

La franja superior indica los dos cortes (operativo y del plan), la vigencia y cuántos indicadores hay en verde, amarillo y rojo.
{% endstep %}

{% step %}
### Estrategia de TI

El mapa estratégico del BSC, la tabla de indicadores con sus metas 2027–2030, el plan real de **22 acciones** en línea de tiempo y los **11 riesgos** (R1 a R11) antes y después de controles.
{% endstep %}

{% step %}
### COBIT

El mapa de los **40 objetivos** por dominio y nivel. Haz clic en **BAI08**: está en nivel 1 y casi no tiene acciones en el plan. Esa es una brecha que el tablero hace visible.
{% endstep %}

{% step %}
### Avanzar el reloj

Como administrador, pulsa **Avanzar un mes** varias veces. Cambian los semáforos, aparecen alertas y el flujo propone subir o bajar niveles. A los aprobadores les llega un correo por dominio con los botones **Aprobar** y **Rechazar**.
{% endstep %}

{% step %}
### Decidir

Abre un botón del correo: el tablero pide iniciar sesión. Entra con el aprobador de ese dominio y confirma. Si lo intenta otra persona, la API no la deja decidir.
{% endstep %}

{% step %}
### Reiniciar

**Reiniciar** vuelve al corte inicial. Los niveles aprobados quedan en el histórico de la corrida anterior; nada se borra.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Qué decir en la presentación:** en la demo el corte es **mensual** para que se vean cambios rápido; en producción sería **trimestral**. Las mediciones son simuladas y no son datos de Addi.
{% endhint %}
