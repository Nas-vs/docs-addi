---
description: Cómo llega el código de GitHub al servidor y qué necesitas antes de empezar.
---

# Cómo se despliega

{% hint style="info" %}
**En una frase:** GitHub Actions construye las imágenes de la aplicación y entra por SSH al servidor para descargarlas y arrancarlas. En el servidor **no se compila nada**.
{% endhint %}

```
merge a main ─► GitHub Actions ─► construye api y web ─► las sube a GHCR
                                         │
                                         └─► SSH a la EC2 ─► docker compose pull + up
                                                           ─► migraciones y carga de datos
```

## Antes de empezar

| Necesitas | Dónde se consigue |
| --- | --- |
| Acceso **admin** al repositorio de código | El dueño del repositorio te lo da en *Settings → Collaborators* |
| Un servidor Linux con Ubuntu | AWS EC2 (`t2.micro` o `t3.micro`), o las alternativas de abajo |
| La llave `.pem` del servidor | Se descarga una sola vez al crear la instancia |
| Un subdominio | [DuckDNS](https://www.duckdns.org), gratis, entrando con GitHub |
| Una cuenta de correo SMTP | [Brevo](https://www.brevo.com), gratis hasta 300 correos al día |
| Una terminal Linux | En Windows, WSL |

## El servidor

{% tabs %}
{% tab title="AWS EC2 (el que usamos)" %}
* **AMI:** Ubuntu Server 24.04 LTS.
* **Tipo:** `t2.micro` o `t3.micro` (1 GB de RAM).
* **Disco:** 20 a 30 GB.
* **Security group:** entrada 22 (SSH), 80 (HTTP) y 443 (HTTPS). Nada más.
* **Elastic IP:** recomendada, para que la IP no cambie si la instancia se reinicia.

Costo fuera de la capa gratuita: unos 13–14 USD al mes encendida todo el tiempo (instancia, IP pública y disco).
{% endtab %}

{% tab title="Google Cloud" %}
* **Máquina:** `e2-micro`, en `us-east1`, `us-central1` o `us-west1`.
* **Disco:** *Standard persistent disk* de 30 GB o menos.
* **Firewall:** marcar *Allow HTTP* y *Allow HTTPS*.
* **Usuario SSH:** crea la llave con `ssh-keygen -t ed25519 -f ~/addi-gcp -C ubuntu` para que el usuario sea `ubuntu`.

En Colombia la cuenta puede pedir un **prepago** para activarse; queda como saldo a favor y se reembolsa al cerrarla si no se usa.
{% endtab %}

{% tab title="Hostinger VPS" %}
* **Plan:** KVM 1 (1 vCPU, 4 GB de RAM), con Ubuntu 24.04.
* **Ubicación:** Brasil (la más cercana a Colombia).
* Entra como `root` y crea el usuario `ubuntu` con sudo antes de seguir la guía.

Es de pago, con descuento si se contrata por varios meses.
{% endtab %}
{% endtabs %}

## Los cuatro pasos

| Paso | Qué haces | Se repite |
| --- | --- | --- |
| [1. Preparar el servidor](preparar-servidor.md) | Dominio, swap, Docker y carpetas | Solo una vez por servidor |
| [2. Secretos y archivo .env](secretos-y-env.md) | Datos de conexión en GitHub y configuración en el servidor | Solo una vez |
| [3. Desplegar y crear usuarios](desplegar.md) | Correr el workflow y crear el admin | El deploy, en cada merge a `main` |
| [4. n8n y correo con Brevo](n8n-y-correo.md) | Importar el flujo y configurar el correo | Solo una vez |
