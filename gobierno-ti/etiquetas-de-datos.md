---
description: Qué significan las etiquetas Real, Estimado y Simulado, y por qué son obligatorias.
---

# Etiquetas de datos

Toda cifra del tablero lleva una etiqueta que dice qué tan confiable es. Es una regla de diseño: **un dato sin etiqueta no se muestra**.

| Etiqueta | Significa | Ejemplo |
| --- | --- | --- |
| **Real** | Sale directamente de una fuente entregada (diagnósticos, BD del BSC) | Certificaciones vigentes: 0 |
| **Estimado** | Se calcula a partir de datos reales con una regla explícita | Madurez global 2,16 (promedio del Diagnóstico de Control) |
| **Simulado** | Lo produce el generador con supuestos aprobados | Disponibilidad mensual, incidentes, avance del plan |

En la interfaz, las etiquetas se distinguen también por su borde: **Real** con fondo sólido, **Estimado** con contorno y **Simulado** con borde punteado.

## La etiqueta se hereda

Cuando el flujo COBIT propone un cambio de nivel, la propuesta hereda **la etiqueta más débil** de los datos que la originaron. Si una propuesta nace de mediciones simuladas, queda como *Simulado*, aunque el nivel actual del objetivo sea *Real*.

{% hint style="warning" %}
Lo simulado demuestra que las reglas del tablero funcionan (semáforos, alertas, propuestas). **No describe cómo está Addi.** Hay que decirlo en cualquier presentación.
{% endhint %}

## Evidencia de los niveles COBIT

El Diagnóstico de Control clasifica la evidencia de cada objetivo:

| Marca | Significa |
| --- | --- |
| **[V]** | Verificada con evidencia pública |
| **[I]** | Inferida |
| **[NV]** | No verificable |

La pestaña COBIT muestra cuántos objetivos tienen cada tipo de evidencia, para distinguir lo demostrable de lo supuesto.
