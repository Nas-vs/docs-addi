---
description: Cómo está organizada esta documentación y cómo editarla con GitBook y Git.
---

# Editar esta documentación

Esta documentación vive en su **propio repositorio** de GitHub y GitBook la publica con **Git Sync**: cada `push` a la rama sincronizada actualiza el sitio, y cada edición hecha en la web de GitBook se guarda como un commit en el repositorio.

## Estructura

{% code title="Repositorio de la documentación" %}
```
.gitbook.yaml          configuración de GitBook (raíz, portada y menú)
README.md              portada
SUMMARY.md             menú lateral: orden y jerarquía de las páginas
.gitbook/assets/       imágenes
inicio/  arquitectura/  gobierno-ti/  uso/  desarrollo-local/
despliegue/  operacion/  referencia/
solucion-de-problemas.md
```
{% endcode %}

## Agregar una página

{% stepper %}
{% step %}
### Crea el archivo

En la carpeta de su sección, con nombre en minúsculas y guiones, por ejemplo `operacion/monitoreo.md`. Empieza con un título `#` y, si quieres, una descripción en el encabezado:

```markdown
---
description: Una frase que resume la página.
---

# Monitoreo
```
{% endstep %}

{% step %}
### Agrégala al menú

En `SUMMARY.md`, en la sección que corresponda:

```markdown
* [Monitoreo](operacion/monitoreo.md)
```

Una página que no está en `SUMMARY.md` no aparece en el sitio.
{% endstep %}

{% step %}
### Sube el cambio

Por Pull Request, igual que el código.
{% endstep %}
{% endstepper %}

## Bloques de GitBook

Escribe los bloques especiales así (los estilos de aviso son `info`, `success`, `warning` y `danger`):

```markdown
{% hint style="info" %}
Texto del aviso.
{% endhint %}

{% tabs %}
{% tab title="Opción A" %}
Contenido A
{% endtab %}
{% tab title="Opción B" %}
Contenido B
{% endtab %}
{% endtabs %}

{% stepper %}
{% step %}
### Primer paso
Contenido
{% endstep %}
{% endstepper %}

<details>
<summary>Título desplegable</summary>
Contenido oculto
</details>
```

## Imágenes

Guárdalas en `.gitbook/assets/` con un nombre descriptivo y enlázalas con ruta relativa:

```html
<figure><img src="../.gitbook/assets/mi-captura.png" alt="Qué muestra"><figcaption><p>Pie de foto</p></figcaption></figure>
```

{% hint style="danger" %}
Antes de subir una captura, revisa que **no muestre contraseñas, tokens, el contenido del `.env` ni la llave `.pem`**.
{% endhint %}
