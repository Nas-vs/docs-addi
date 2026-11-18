---
description: Cómo se respalda la base de datos y cómo restaurar un respaldo.
---

# Respaldos

## Qué se respalda

Todos los días a las **3:30 a. m.** (hora del servidor) el script `backup.sh` guarda las dos bases:

| Base | Contiene |
| --- | --- |
| `addi` | Datos del tablero, usuarios, aprobaciones, histórico de madurez y auditoría |
| `n8n` | El flujo, sus credenciales (cifradas) y el historial de ejecuciones |

Los respaldos quedan en `/opt/addi-dashboard/backups/` y se conservan **7 días**.

{% hint style="warning" %}
Los respaldos están **en el mismo servidor**. Si se borra la instancia, se pierden. Antes de terminar una instancia o hacer un cambio grande, descarga una copia a tu PC.
{% endhint %}

## Comandos útiles

{% tabs %}
{% tab title="Respaldar ahora" %}
```bash
cd /opt/addi-dashboard
bash backup.sh
ls -lh backups/
```
{% endtab %}

{% tab title="Descargar a tu PC" %}
Desde tu terminal (no desde el servidor):

```bash
scp -i ~/.ssh/addi-ti.pem "ubuntu@<IP>:/opt/addi-dashboard/backups/*.dump" ./respaldos/
```
{% endtab %}

{% tab title="Restaurar" %}
```bash
cd /opt/addi-dashboard
set -a; . ./.env; set +a
docker compose stop api n8n
docker compose exec -T postgres pg_restore -U "$POSTGRES_SUPERUSER" -d addi --clean --if-exists < backups/addi-AAAA-MM-DD.dump
docker compose start api n8n
```

Cambia `AAAA-MM-DD` por la fecha del respaldo. Para n8n, usa `-d n8n` y el archivo `n8n-…dump`.
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
Restaurar **reemplaza** los datos actuales por los del respaldo. Haz primero un respaldo del estado actual por si necesitas volver.
{% endhint %}

## Revisar que el respaldo corre

```bash
tail -n 5 /opt/addi-dashboard/backups/backup.log
```

Cada día debe aparecer una línea con `respaldo ok`.
