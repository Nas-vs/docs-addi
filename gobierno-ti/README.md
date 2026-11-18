---
description: Qué marcos usa el tablero, de dónde salen sus cifras y cómo se relacionan.
---

# COBIT 2019 y BSC en el tablero

El tablero une dos marcos que responden preguntas distintas:

| Marco | Pregunta que responde | En el tablero |
| --- | --- | --- |
| **COBIT 2019** | ¿Qué tan maduros son los procesos de gobierno y gestión de TI? | 40 objetivos en 5 dominios, cada uno con nivel de capacidad 0 a 5 |
| **BSC de TI 2027–2030** | ¿TI está logrando lo que la estrategia necesita? | 60 indicadores en 4 perspectivas, con metas por año |

Los dos se conectan por la tabla `vinculos_objetivo`: cada objetivo COBIT se liga a los indicadores, acciones del plan y riesgos que lo mueven. Así, si un indicador lleva dos cortes en rojo, el tablero sabe qué objetivo COBIT afecta.

## Fuentes

| Fuente | Qué aporta | Tipo |
| --- | --- | --- |
| Diagnóstico de Control (COBIT) | Nivel de capacidad de cada objetivo y su evidencia ([V] verificada, [I] inferida, [NV] no verificable) | Real |
| Diagnóstico de Awareness (COBIT) | Importancia y desempeño percibido de los 40 objetivos | Real |
| Autoevaluación de gobierno de TI | Puntaje 35 de 62, nivel 2 | Real |
| BD del BSC | Indicadores, metas 2027–2030, plan de 22 acciones y 11 riesgos | Real |
| Generador de datos | Mediciones mensuales de cada indicador | Simulado |

## Dominios COBIT

| Dominio | Qué cubre | Nivel promedio |
| --- | --- | --- |
| EDM | Evaluar, dirigir y monitorear | 2,60 |
| APO | Alinear, planificar y organizar | 2,08 |
| BAI | Construir, adquirir e implementar | 1,64 |
| DSS | Entregar, dar servicio y soporte | 2,67 |
| MEA | Monitorear, evaluar y valorar | 2,67 |

El promedio global es **2,16**, con **6 objetivos en nivel 1**. BAI es el dominio más bajo: varios de sus procesos de gestión interna (conocimiento, cambio organizacional, programas) están en nivel 1.

## Perspectivas del BSC

* **Financiera**: costo y presupuesto de TI.
* **Cliente**: lo que perciben comercios, aliados y usuarios (disponibilidad, integraciones).
* **Procesos internos**: seguridad, datos, continuidad, entrega de software.
* **Aprendizaje y crecimiento**: personas, conocimiento y cultura de ejecución.

## Supuestos del modelo

El generador trabaja con 12 supuestos aprobados (S1 a S12). Los más importantes para leer el tablero:

| Supuesto | Valor |
| --- | --- |
| S1 · Fecha de corte | Dos relojes: operativo en 2026-11 y del plan en 2028-06 |
| S2 · Metas | Metas reales de la BD del BSC para 2027 a 2030 |
| S5 · Nivel objetivo de madurez | Importancia 4 o 5 → nivel 3; importancia 3 → nivel 2; nunca menor al actual |
| S6 · Apetito de riesgo | Seguridad y datos: bajo · Cumplimiento: bajo · Operación: bajo a medio · Innovación: medio |
| S9 · RTO objetivo | 4 horas |

{% hint style="info" %}
La lista completa de supuestos aparece en la pestaña **Estrategia de TI** del tablero.
{% endhint %}

## Sigue leyendo

* [Semáforos y metas](semaforos-y-metas.md)
* [Etiquetas de datos](etiquetas-de-datos.md)
* [Flujo de propuestas COBIT](flujo-de-propuestas.md)
