---
description: Las reglas que no se rompen y por qué el sistema se construyó así.
---

# Decisiones de diseño

## Reglas que no se rompen

1. **No se compila nada en el servidor.** GitHub Actions construye las imágenes; la instancia solo descarga y arranca.
2. **Swap de 2 GB y límite de memoria por contenedor.**
3. **Un solo PostgreSQL** para la aplicación y n8n, en bases y usuarios separados.
4. **Nunca se edita a mano un dato generado.** Se cambia un parámetro, se regenera y se sube `db/`.
5. **Toda cifra lleva su etiqueta**: Real, Estimado o Simulado.
6. **El flujo propone y una persona aprueba.** El correo trae los botones; la decisión exige sesión y rol.

## Por qué así

| Decisión | Alternativa descartada | Motivo |
| --- | --- | --- |
| Solo la API escribe en la base | n8n con usuario y contraseña de Postgres | Si n8n se compromete, no puede modificar datos. Hay un solo punto de control y auditoría. |
| Aprobar exige iniciar sesión | El enlace del correo registra la decisión | Un correo reenviado o filtrado permitiría aprobar a cualquiera. Ahora la API verifica que seas el titular o el suplente. |
| Un flujo de n8n en vez de tres | Flujos para leer el tablero, el corte y las decisiones | Leer el tablero y decidir pasaron a la API con login. n8n solo programa el corte y envía correos. |
| Las reglas viven en SQL | Reglas en Python o en n8n | Las reglas de la v1 (semáforos, propuestas, caducidad) ya estaban probadas en SQL. La API las llama sin reescribirlas. |
| Imágenes construidas en GitHub Actions | `docker build` en la EC2 | Una instancia de 1 GB de RAM se queda sin memoria compilando React. Además, lo que se despliega es exactamente lo que pasó la CI. |
| Generador con semilla fija y huella | Datos aleatorios en cada ejecución | Cualquiera puede regenerar y obtener exactamente los mismos datos. La CI lo verifica. |
| Caddy para HTTPS | Nginx + Certbot | Caddy saca y renueva el certificado solo, con menos configuración y menos memoria. |
| Editor de n8n por túnel SSH | Editor publicado en internet | El editor puede ejecutar código y guarda las credenciales de correo. Publicarlo sería una puerta de entrada al servidor. |

## Qué conserva de la versión 1

La lógica de negocio es la de la v1 de Julian: el generador, el esquema `addi`, las funciones SQL (`api_dashboard`, `fn_semaforo`, `fn_proponer_cambios`, `fn_decidir`, `fn_avanzar_corte`…), los dos relojes y el histórico inmutable de niveles. La integración agregó login, aprobación controlada, la interfaz en React y el despliegue.

{% hint style="info" %}
El detalle de qué se conservó, qué cambió y qué está por confirmar está en `INTEGRACION.md` del repositorio de código.
{% endhint %}
