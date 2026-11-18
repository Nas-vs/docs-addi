---
description: "El panel de administración: reloj, n8n, aprobadores, usuarios y auditoría."
---

# Administración

La pestaña **Administración** solo la ven los usuarios con rol admin.

## Reloj de la demo

| Botón | Qué hace |
| --- | --- |
| **Avanzar un mes** | Mueve los dos relojes un mes, recalcula semáforos y alertas, aplica las reglas de propuesta y pide a n8n que envíe los correos. Si faltan mediciones para el mes nuevo, no avanza. |
| **Pausar / Reanudar** | Detiene el reloj. Ni el botón ni el cron de n8n lo mueven mientras está pausado. |
| **Reiniciar** | Vuelve al corte inicial y abre una corrida nueva. Los niveles aprobados en la corrida anterior quedan en el histórico. |

{% hint style="info" %}
El reloj es **idempotente**: si dos personas (o el cron y una persona) lo avanzan al mismo tiempo, solo se avanza una vez.
{% endhint %}

## Conexión con n8n

Muestra si n8n responde y si el flujo está activo, junto con las últimas ejecuciones del corte y de los avisos.

| Estado | Significa | Qué hacer |
| --- | --- | --- |
| 🟢 **n8n conectado, flujo activo** | Todo bien | — |
| **n8n conectado, flujo inactivo** | n8n funciona, pero el flujo no está importado o publicado | [Configurar n8n](../despliegue/n8n-y-correo.md) |
| **n8n no responde** | El contenedor está caído o reiniciando | Revisar `docker compose logs n8n` |

## Aprobadores por dominio

Define el **titular** y el **suplente** de cada dominio COBIT. Deben ser personas distintas y con rol aprobador.

## Usuarios

* **Crear:** nombre, cargo, correo y roles. El sistema genera una **contraseña temporal** que se muestra una sola vez.
* **Editar:** cambiar nombre, cargo, correo, roles o desactivar al usuario.
* **Restablecer:** genera una contraseña temporal nueva.

## Auditoría

Registro de quién hizo qué y cuándo: inicios de sesión, decisiones, avances del reloj y cambios de usuarios o aprobadores. No se puede editar desde el tablero.
