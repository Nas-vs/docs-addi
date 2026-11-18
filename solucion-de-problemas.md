---
description: Los errores más comunes al desplegar y operar el tablero, con su causa y solución.
---

# Solución de problemas

Abre el mensaje que estás viendo.

## Al conectarse al servidor

<details>

<summary><code>Permission denied (publickey)</code></summary>

**Causa:** falta `ubuntu@` en el comando, o la ruta o el nombre de la llave están mal.

**Solución:**

```bash
ls ~/.ssh/                       # mira cómo se llama tu llave
chmod 400 ~/.ssh/<tu-llave>.pem
ssh -i ~/.ssh/<tu-llave>.pem ubuntu@<IP>
```

</details>

<details>

<summary><code>Identity file … not accessible: No such file or directory</code></summary>

**Causa:** la llave no está en esa ruta. Puede que estés en otro computador o con otro usuario.

**Solución:** búscala y cópiala a `~/.ssh/`:

```bash
find ~ /mnt/c/Users/*/Downloads -name "*.pem" 2>/dev/null
```

</details>

<details>

<summary><code>REMOTE HOST IDENTIFICATION HAS CHANGED</code></summary>

**Causa:** esa IP ahora pertenece a otra instancia (por ejemplo, reutilizaste la Elastic IP).

**Solución:**

```bash
ssh-keygen -R <IP>
```

</details>

## Al preparar el servidor

<details>

<summary><code>/opt/addi-dashboard/.env: Permission denied</code></summary>

**Causa:** la carpeta pertenece a `root`.

**Solución:**

```bash
sudo chown -R $USER:$USER /opt/addi-dashboard
```

</details>

<details>

<summary><code>no configuration file provided: not found</code></summary>

**Causa:** corriste `docker compose` fuera de la carpeta de la aplicación.

**Solución:**

```bash
cd /opt/addi-dashboard
```

</details>

## En el despliegue (GitHub Actions)

<details>

<summary><code>failed to read /opt/addi-dashboard/.env: line 2: unexpected character "("</code></summary>

**Causa:** el bloque que crea el `.env` se pegó dentro de un editor (`nano`) en vez de ejecutarse en la terminal. El archivo quedó con el texto del script.

**Solución:** borra el archivo y vuelve a pegar el bloque de [Secretos y archivo .env](despliegue/secretos-y-env.md) directamente en la terminal.

```bash
rm /opt/addi-dashboard/.env
```

</details>

<details>

<summary>El paso <code>desplegar</code> falla al conectar por SSH</summary>

**Causa:** el secreto `EC2_HOST` tiene una IP vieja, `EC2_SSH_KEY` está incompleto, o el security group no deja entrar a GitHub por el puerto 22.

**Solución:** revisa los secretos (la llave debe incluir las líneas `-----BEGIN` y `-----END`). Las IP de GitHub Actions cambian, así que el puerto 22 debe aceptar conexiones desde cualquier lugar, protegido solo con llave.

</details>

<details>

<summary>Falla el <code>docker login</code> o el <code>pull</code> de las imágenes</summary>

**Causa:** el `GHCR_PAT` venció o no tiene el permiso `read:packages`.

**Solución:** genera un token nuevo y actualiza el secreto.

</details>

## Al abrir el sitio

<details>

<summary>El sitio no carga y Caddy muestra <code>Timeout during connect (likely firewall problem)</code></summary>

**Causa:** DuckDNS apunta a una IP vieja, o los puertos 80 y 443 están cerrados. El log dice a qué IP intentó conectarse Let's Encrypt; compárala con la del servidor:

```bash
curl -s ifconfig.me; echo
```

**Solución:** actualiza la IP en DuckDNS (y en `EC2_HOST`), revisa el security group y reinicia Caddy:

```bash
docker compose restart web
```

</details>

<details>

<summary>Carga la página pero no deja iniciar sesión</summary>

**Causa:** estás entrando por `http://` o por la IP. Con `ENV=produccion` la sesión solo funciona por **HTTPS**.

**Solución:** entra siempre por `https://<dominio>`.

</details>

<details>

<summary><code>Demasiados intentos. Espera un minuto.</code></summary>

**Causa:** más de 5 intentos de inicio de sesión en un minuto desde la misma IP.

**Solución:** espera un minuto. Si olvidaste la contraseña, un admin la restablece en **Administración → Usuarios**.

</details>

## n8n y correos

<details>

<summary>"n8n conectado, flujo inactivo"</summary>

**Causa:** el flujo no está importado o no está publicado.

**Solución:** sigue [n8n y correo con Brevo](despliegue/n8n-y-correo.md) e importa y publica el flujo.

</details>

<details>

<summary>n8n está vacío: sin flujos ni base de datos</summary>

Es normal la primera vez. El flujo se importa a mano una sola vez, y n8n **no** necesita credenciales de base de datos: habla con la API.

</details>

<details>

<summary>El correo no llega</summary>

1. Revisa la carpeta de **spam**.
2. En Brevo, **Transactional → Logs**: dice si se entregó o por qué se bloqueó.
3. `CORREO_REMITENTE` debe ser **exactamente** el remitente verificado en Brevo:

```bash
docker compose exec n8n printenv CORREO_REMITENTE
```

4. El aprobador debe tener un correo real (los `@addi.demo` no existen).
5. En n8n, pestaña **Executions**: el nodo en rojo dice el error.

</details>

<details>

<summary><code>Authentication unsuccessful, basic authentication is disabled</code></summary>

**Causa:** intentaste enviar con Hotmail u Outlook directamente. Microsoft ya no permite usuario y contraseña por SMTP en cuentas personales.

**Solución:** usa Brevo.

</details>

## Base de datos

<details>

<summary><code>password authentication failed</code> en los logs de api o n8n</summary>

**Causa:** se regeneró el `.env` después de crear la base. Las contraseñas nuevas no coinciden con las guardadas.

**Solución:** si tienes el `.env` anterior, restáuralo. Si no y la base se puede perder, bórrala y vuelve a desplegar:

```bash
docker compose down -v
```

Luego vuelve a correr el workflow Deploy y a crear los usuarios.

{% hint style="danger" %}
`down -v` borra todos los datos: decisiones, usuarios y configuración de n8n.
{% endhint %}

</details>

{% hint style="info" %}
¿No está tu error? Revisa los logs del servicio en [Logs y estado](operacion/logs-y-estado.md).
{% endhint %}
