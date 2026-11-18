---
description: Endpoints de la API, quién puede usarlos y cómo se autentican.
---

# API

Todas las rutas empiezan por `/api/v1`. Los errores siempre tienen el mismo formato:

```json
{ "error": { "codigo": "sin_sesion", "mensaje": "Inicia sesión." } }
```

## Autenticación

* **Sesión:** cookie `httpOnly` que se crea al iniciar sesión.
* **CSRF:** toda petición que cambia algo (`POST`, `PUT`, `PATCH`) debe enviar la cabecera `X-CSRF-Token` con el valor de la cookie `csrf`.
* **Interna:** las rutas `/internal` exigen la cabecera `X-Internal-Token`. Caddy las bloquea desde internet; solo las usa n8n por la red interna.

## Rutas

{% tabs %}
{% tab title="Sesión" %}
| Método | Ruta | Quién | Qué hace |
| --- | --- | --- | --- |
| `POST` | `/auth/login` | Todos | Inicia sesión (máximo 5 intentos por minuto) |
| `POST` | `/auth/logout` | Con sesión | Cierra la sesión |
| `GET` | `/auth/me` | Con sesión | Perfil, roles y dominios que aprueba |
| `POST` | `/auth/cambiar-contrasena` | Con sesión | Cambia la contraseña (mínimo 10 caracteres) |
| `GET` | `/salud` | Todos | Comprueba que la API responde |
{% endtab %}

{% tab title="Tablero" %}
| Método | Ruta | Quién | Qué hace |
| --- | --- | --- | --- |
| `GET` | `/tablero` | Con sesión | JSON completo del tablero. Responde `503 sin_datos` si la base no tiene datos cargados |
{% endtab %}

{% tab title="Aprobaciones" %}
| Método | Ruta | Quién | Qué hace |
| --- | --- | --- | --- |
| `GET` | `/aprobaciones` | Aprobador | Propuestas pendientes y decididas de mis dominios |
| `GET` | `/aprobaciones/{id}` | Aprobador | Detalle con indicadores, acciones y riesgos ligados, y si puedo decidir |
| `POST` | `/aprobaciones/{id}/aprobar` | Titular o suplente escalado | Aprueba y crea la versión nueva del nivel |
| `POST` | `/aprobaciones/{id}/rechazar` | Titular o suplente escalado | Rechaza. Exige `motivo` |
{% endtab %}

{% tab title="Administración" %}
| Método | Ruta | Qué hace |
| --- | --- | --- |
| `GET` | `/admin/reloj` | Estado del reloj y últimas ejecuciones |
| `POST` | `/admin/reloj/avanzar` | Avanza un mes y pide a n8n enviar los correos |
| `POST` | `/admin/reloj/reiniciar` | Vuelve al corte inicial |
| `POST` | `/admin/reloj/pausa` | Pausa o reanuda el reloj |
| `GET` | `/admin/n8n` | Estado de n8n y del flujo |
| `GET` / `POST` | `/admin/usuarios` | Lista o crea usuarios (devuelve contraseña temporal) |
| `PATCH` | `/admin/usuarios/{id}` | Edita usuario, roles o lo desactiva |
| `POST` | `/admin/usuarios/{id}/restablecer` | Nueva contraseña temporal |
| `GET` | `/admin/aprobadores` | Titular y suplente por dominio |
| `PUT` | `/admin/aprobadores/{dominio}` | Cambia titular y suplente |
| `GET` | `/admin/auditoria` | Registro de auditoría |

Todas exigen rol **admin**.
{% endtab %}

{% tab title="Interna (n8n)" %}
| Método | Ruta | Qué hace |
| --- | --- | --- |
| `POST` | `/internal/corte` | Avanza el corte (idempotente por mes) |
| `GET` | `/internal/avisos` | Devuelve los correos listos para enviar: destinatario, copia, asunto, HTML y texto |

Exigen `X-Internal-Token`. No son accesibles desde internet.
{% endtab %}
{% endtabs %}

## Códigos de error frecuentes

| HTTP | `codigo` | Significa |
| --- | --- | --- |
| 401 | `credenciales` | Correo o contraseña incorrectos |
| 401 | `sin_sesion` / `sesion_invalida` / `sesion_revocada` | Falta iniciar sesión o la sesión venció |
| 403 | `no_puede_decidir` | No eres titular ni suplente escalado de ese dominio, o no tienes el rol |
| 409 | `no_puede_decidir` | La propuesta ya se decidió |
| 409 | `no_aplicada` | La propuesta caducó porque el nivel cambió |
| 422 | `falta_motivo` | Rechazar exige escribir el motivo |
| 429 | `demasiados_intentos` | Muchos intentos de inicio de sesión |
| 503 | `sin_datos` | La base no tiene los datos cargados |

{% hint style="info" %}
La documentación interactiva está en http://localhost:8000/api/docs cuando corres el proyecto en local. En producción Caddy la bloquea.
{% endhint %}
