---
description: Los tres roles del tablero y qué puede hacer cada uno.
---

# Roles y permisos

Todo el tablero exige **iniciar sesión**. Cada usuario tiene uno o varios roles:

| Rol | Puede | No puede |
| --- | --- | --- |
| **Lector** | Ver todas las pestañas del tablero y las fichas de indicadores | Decidir propuestas ni cambiar nada |
| **Aprobador** | Lo del lector + ver *Mis aprobaciones* y decidir las propuestas de **sus** dominios | Decidir propuestas de otros dominios |
| **Admin** | Avanzar, pausar y reiniciar el reloj · gestionar usuarios y aprobadores · ver la auditoría y el estado de n8n | Aprobar propuestas solo por ser admin |

{% hint style="warning" %}
**El admin no aprueba por ser admin.** Para decidir una propuesta hay que tener el rol aprobador **y** ser el titular o el suplente del dominio. Así se separa quien opera el sistema de quien juzga la madurez.
{% endhint %}

## Seguridad de la sesión

* La sesión dura **8 horas** (`SESSION_HOURS`). Después hay que volver a entrar.
* La cookie de sesión es `httpOnly` (el código de la página no la puede leer) y en producción solo viaja por HTTPS.
* Cada acción que cambia algo exige además un token **CSRF**.
* Hay un máximo de **5 intentos de inicio de sesión por minuto** desde la misma IP.
* Las contraseñas se guardan con **argon2** y deben tener al menos 10 caracteres.
* Toda decisión y acción de administración queda en la **auditoría**, con usuario, fecha e IP.

## Usuarios de la demostración

El comando `crear-usuarios-demo` crea un aprobador por dominio y un lector:

| Correo | Rol | Dominio |
| --- | --- | --- |
| `cto@addi.demo` | Aprobador | EDM (y suplente de APO, BAI, DSS, MEA) |
| `apo@addi.demo` | Aprobador | APO |
| `bai@addi.demo` | Aprobador | BAI |
| `dss@addi.demo` | Aprobador | DSS |
| `mea@addi.demo` | Aprobador | MEA (y suplente de EDM) |
| `lector@addi.demo` | Lector | — |

{% hint style="info" %}
Las direcciones `@addi.demo` no existen. Para que lleguen los correos, cámbialas por correos reales en **Administración → Usuarios**.
{% endhint %}
