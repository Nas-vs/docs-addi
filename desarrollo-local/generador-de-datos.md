---
description: >-
  Cómo funciona el generador de datos en Python, cómo regenerar y cómo cambiar
  un parámetro sin romper la trazabilidad.
---

# Generador de datos

El generador produce los scripts SQL que cargan la base: el esquema, los datos base y las mediciones mensuales de cada indicador.

{% hint style="warning" %}
**Nunca se edita a mano un archivo de `db/`.** Se cambia un parámetro, se regenera y se suben los cambios. La CI rechaza el Pull Request si `db/` no coincide con lo que produce el generador.
{% endhint %}

## Qué hay en la carpeta

| Archivo | Para qué |
| --- | --- |
| `extraer_diagnosticos.py` | Lee los tres Excel de diagnóstico COBIT de `datos/` |
| `extraer_bsc.py` | Lee la BD del BSC y escribe `bsc.json` (plan, riesgos, metas) |
| `catalogo.py` | Catálogo de indicadores y vínculos con COBIT (`PLAN_VINCULOS`, `RIESGOS_VINCULOS`) |
| `parametros.json` | Supuestos y parámetros del modelo, con su `version` |
| `generar.py` | Genera `db/02` y `db/03` y valida el resultado |
| `huella.txt` | Huella SHA-256 de lo generado |

## Cómo garantiza que sea replicable

* Cada indicador y mes tiene una **semilla fija**: los mismos parámetros dan exactamente los mismos números.
* `generar.py` **genera dos veces** y compara las huellas.
* Valida rangos y coherencia. Si algo falla, **no escribe nada**.

## Regenerar

```bash
cd generador
pip install numpy openpyxl
python3 extraer_diagnosticos.py ../datos   # solo si cambian los Excel de diagnóstico
python3 extraer_bsc.py                     # solo si cambia la BD del BSC
python3 generar.py                         # reescribe db/02 y db/03 e imprime la huella
```

## Cambiar un parámetro

{% stepper %}
{% step %}
### Edita el parámetro

En `parametros.json` (o en `catalogo.py` si es un vínculo con COBIT).
{% endstep %}

{% step %}
### Sube la versión

Cambia `version` en `parametros.json`, por ejemplo de `v1.1` a `v1.2`.
{% endstep %}

{% step %}
### Regenera y sube los cambios

```bash
python3 generar.py
git add generador/ db/
git commit -m "feat(datos): <qué cambió y por qué>"
```
{% endstep %}
{% endstepper %}

Al desplegar, el servidor solo recarga los datos si la versión cambió (`cargar-datos --si-cambio`).

## Cómo se simula una medición

El valor de cada mes combina varias piezas, todas documentadas en los supuestos:

* **Tendencia** hacia la meta según el avance del plan.
* **Volumen** del negocio V(t), con estacionalidad (campaña de noviembre, cierres de la SFC).
* **Factor de madurez** M = 1,6 − 0,3 × nivel promedio de los procesos: más madurez, menos incidentes.
* **Ruido** entre 1 % y 20 % según el indicador.
* **Eventos** del calendario, como el incidente del 25 de marzo de 2026.

## Pendientes de confirmar

<details>

<summary>Vínculos de acciones y riesgos con COBIT</summary>

La BD del BSC no trae a qué objetivo COBIT responde cada acción y cada riesgo. Son una propuesta del equipo técnico, marcada *por confirmar* en el tablero. Se corrigen en `catalogo.py`.

</details>

<details>

<summary>Metas frente a parámetros del generador</summary>

Con las metas reales, C3.1 baja a 40 en 2030 y C1.1 a 60, pero el generador apunta a 85. PI2.2 tiene meta 100 y techo 85. Esos indicadores terminarán en rojo hasta que el equipo decida si ajusta el parámetro o la meta.

</details>

<details>

<summary>Peso de cada acción en el presupuesto</summary>

Hoy es proporcional a la duración de la acción. Falta el peso real.

</details>
