---
description: Términos de gobierno de TI y de infraestructura usados en esta documentación.
---

# Glosario

## Gobierno de TI

| Término | Significado |
| --- | --- |
| **COBIT 2019** | Marco de ISACA para el gobierno y la gestión de la TI empresarial. Organiza 40 objetivos en 5 dominios |
| **Dominio** | Grupo de objetivos COBIT: EDM, APO, BAI, DSS y MEA |
| **Objetivo COBIT** | Proceso de gobierno o gestión, por ejemplo APO13 (Seguridad) o BAI08 (Conocimiento) |
| **Nivel de capacidad** | Madurez de un proceso de 0 (incompleto) a 5 (optimizado) |
| **BSC** | *Balanced Scorecard*: indicadores en cuatro perspectivas (Financiera, Cliente, Procesos internos, Aprendizaje y crecimiento) |
| **Corte** | Fecha en la que se toman las mediciones y se recalcula el tablero |
| **Meta a la fecha** | Meta interpolada entre dos metas anuales según el mes del corte |
| **Propuesta** | Cambio de nivel sugerido por el flujo, pendiente de que una persona lo apruebe |
| **Titular / Suplente** | Persona que aprueba los cambios de un dominio / quien la reemplaza si no responde |
| **Escalar** | Habilitar al suplente cuando una propuesta lleva varios cortes sin respuesta |
| **Caducada** | Propuesta que ya no aplica porque el nivel vigente cambió |
| **Corrida** | Una ejecución de la demo desde el corte inicial. Reiniciar abre una nueva |
| **[V] / [I] / [NV]** | Evidencia verificada / inferida / no verificable |
| **RTO / RPO** | Tiempo máximo para recuperar un servicio / pérdida máxima de datos aceptable |
| **SFC** | Superintendencia Financiera de Colombia |

## Infraestructura

| Término | Significado |
| --- | --- |
| **EC2** | Servidor virtual de AWS |
| **Elastic IP** | IP pública fija de AWS que no cambia al reiniciar la instancia |
| **Security group** | Firewall de AWS que define qué puertos están abiertos |
| **Swap** | Espacio en disco que se usa como memoria cuando se acaba la RAM |
| **Docker Compose** | Herramienta que arranca varios contenedores con un solo archivo |
| **GHCR** | GitHub Container Registry: donde se guardan las imágenes construidas |
| **GitHub Actions** | Automatización de GitHub que corre la CI y el despliegue |
| **CI** | Integración continua: pruebas automáticas en cada cambio |
| **Caddy** | Servidor web que saca y renueva solo los certificados HTTPS |
| **DuckDNS** | Servicio gratuito de subdominios dinámicos |
| **Túnel SSH** | Conexión cifrada que lleva un puerto del servidor a tu computador |
| **SMTP** | Protocolo para enviar correos |
| **Brevo** | Servicio de envío de correos con SMTP gratuito |
| **n8n** | Herramienta de automatización de flujos |
| **CSRF** | Ataque que aprovecha una sesión abierta; se previene con un token adicional |
| **Idempotente** | Que repetir la operación no cambia el resultado más de una vez |
