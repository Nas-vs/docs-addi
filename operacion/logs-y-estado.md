---
description: Cómo saber si todo está funcionando y dónde mirar cuando algo falla.
---

# Logs y estado

Todos los comandos se corren en el servidor, **dentro de** `/opt/addi-dashboard`.

## Estado general

```bash
cd /opt/addi-dashboard
docker compose ps
free -h
df -h /
```

| Qué mirar | Bien | Mal |
| --- | --- | --- |
| `docker compose ps` | Los 4 servicios en `Up`, postgres `healthy` | Alguno en `Restarting` o `Exited` |
| `free -h` | Swap usada por debajo de 1 GB | Swap casi llena: hay que revisar qué consume |
| `df -h /` | Uso de disco por debajo del 80 % | Disco lleno: limpia imágenes viejas con `docker image prune -f` |

## Logs por servicio

{% tabs %}
{% tab title="api" %}
```bash
docker compose logs --tail=60 api
```

Al arrancar debe mostrar `Application startup complete`. Los errores de la aplicación salen aquí.
{% endtab %}

{% tab title="web (Caddy)" %}
```bash
docker compose logs --tail=40 web
```

Útil para problemas de HTTPS. Busca `certificate obtained successfully` o errores de `challenge`.
{% endtab %}

{% tab title="n8n" %}
```bash
docker compose logs --tail=40 n8n
```

Para ver por qué falló un correo es mejor la pestaña **Executions** del editor de n8n (por túnel).
{% endtab %}

{% tab title="postgres" %}
```bash
docker compose logs --tail=30 postgres
```

Si ves `password authentication failed`, el `.env` cambió después de crear la base.
{% endtab %}
{% endtabs %}

Para seguir un log en vivo, agrega `-f` y sal con `Ctrl+C`:

```bash
docker compose logs -f api
```

## Desde el tablero

* **Administración → Conexión con n8n:** si n8n responde, si el flujo está activo y las últimas ejecuciones del corte y los avisos.
* **Administración → Auditoría:** quién hizo qué y cuándo.
* `https://<dominio>/api/v1/salud`: responde si la API está viva.

## En Brevo

**Transactional → Logs** muestra cada correo: entregado, diferido o bloqueado, con el motivo.
