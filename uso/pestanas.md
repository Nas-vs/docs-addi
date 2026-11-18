---
description: Qué encuentras en cada pestaña del tablero y cómo navegarlo.
---

# Pestañas del tablero

## La barra superior

Siempre visible, arriba de todo:

* **Buscador** de indicadores o responsables.
* **Filtros** por perspectiva del BSC y por tipo de dato (Real, Estimado, Simulado).
* **Franja de estado:** los dos cortes (operativo y del plan), la vigencia de los datos y los contadores de indicadores en verde, amarillo y rojo.
* Para administradores: los botones **Avanzar un mes** y **Reiniciar**, y el estado de la conexión con n8n.

{% hint style="info" %}
Al hacer clic en cualquier indicador se abre su **ficha** en un panel lateral: definición, fórmula, responsable, fuente, histórico, meta por año y etiqueta. La dirección de la página cambia (`?ind=…`), así que puedes compartir el enlace directo a esa ficha.
{% endhint %}

## Las pestañas

{% tabs %}
{% tab title="General" %}
El **resumen ejecutivo** para el CIO:

* Madurez global de TI y radar por dominio COBIT.
* Semáforos por perspectiva del BSC.
* **Qué requiere una decisión:** alertas con impacto, decisión sugerida, responsable y fecha límite.
* **Lo más importante por tema:** un resumen de Datos, Seguridad, IA, Plan, Riesgos y Operación.
{% endtab %}

{% tab title="Estrategia de TI" %}
* Mapa estratégico del BSC de TI.
* Tabla de indicadores del BSC con metas 2027–2030.
* Diagnóstico situacional del BSC.
* **Plan de acción:** las 22 acciones en línea de tiempo por ruta y su avance frente al presupuesto.
* **Riesgos:** matriz de riesgo inherente y registro de los 11 riesgos con dueño, proceso COBIT y señal.
* Supuestos del modelo y calendario de eventos.
{% endtab %}

{% tab title="Datos" %}
* Madurez en el manejo de datos por área.
* Gobierno de datos: datos críticos con dueño.
* Reportes a la SFC que cuadran.
{% endtab %}

{% tab title="Seguridad" %}
* Protección de la información (datos sensibles cifrados).
* Incidentes del mes por gravedad y tiempos de respuesta.
* Certificaciones (ISO 27001, PCI DSS) y respaldo formal de la seguridad.
* Solicitudes de titulares atendidas a tiempo (Ley 1581).
{% endtab %}

{% tab title="IA" %}
* Modelos de IA en uso (scoring de crédito, antifraude y biometría, asistente LLM).
* Para cada modelo: **¿puede salir a producción?** Sí, con condiciones o no, según estabilidad, desempeño, correcciones y explicabilidad.
* Madurez de IA por dimensión e incidentes de IA.
{% endtab %}

{% tab title="Operación" %}
* Disponibilidad de la plataforma y continuidad.
* Entrega de software: despliegues, cambios fallidos y tiempo de la idea a producción.
* Costo de nube y TI frente al presupuesto.
* Equipo de TI.
{% endtab %}

{% tab title="COBIT" %}
* Mapa de los **40 objetivos** con su nivel y la brecha por dominio.
* **Dónde poner la atención:** importancia contra desempeño y objetivos en alerta.
* Calidad de la evidencia: cobertura [V]/[I]/[NV] y formales frente a auditados.
* Autoevaluación por sección y madurez proyectada 2026–2030.
* **Cola de cambios propuestos** e **historial de cambios de nivel**.
* Cómo funciona el flujo y quién aprueba cada dominio.

Al hacer clic en un objetivo se ve su detalle: indicadores, acciones del plan y riesgos ligados.
{% endtab %}
{% endtabs %}

## Pestañas según tu rol

| Pestaña | Lector | Aprobador | Admin |
| --- | :---: | :---: | :---: |
| General, Estrategia, Datos, Seguridad, IA, Operación, COBIT | ✅ | ✅ | ✅ |
| Mis aprobaciones | — | ✅ | Solo si también es aprobador |
| Administración | — | — | ✅ |
| Cuenta (cambiar contraseña) | ✅ | ✅ | ✅ |
