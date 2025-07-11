# Infraestructura de Monitoreo con Docker

Este repositorio contiene un stack completo de monitoreo desplegado con Docker Compose, orientado a proyectos desplegados en contenedores. La infraestructura incluye métricas, logs, alertas y monitoreo de endpoints externos.

---

## 🧱 Servicios incluidos

| Servicio           | Descripción                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| **Prometheus**     | Motor de recolección de métricas para infraestructura y servicios.          |
| **Grafana**        | Visualización de métricas y logs. Dashboards persistentes.                  |
| **Loki**           | Almacenamiento de logs.                                                     |
| **Promtail**       | Recolector de logs del sistema (syslog, contenedores, etc).                 |
| **Alertmanager**   | Gestión de alertas disparadas por Prometheus.                               |
| **Blackbox Exporter** | Chequeos HTTP(S)/TCP/ICMP a endpoints externos (URLs, APIs, etc).       |
| **Node Exporter**  | Exporta métricas del sistema operativo (CPU, RAM, disco, etc).              |
| **cAdvisor**       | Métricas de uso de recursos de contenedores Docker.                         |

---

## 🐳 Estructura del proyecto

infra-monitoring/
├── docker-compose.yml
├── prometheus/
│ └── prometheus.yml
├── grafana/
│ ├── data/ # Volumen persistente (dashboards, usuarios, config)
│ └── provisioning/ # Provisionamiento opcional
├── loki/
│ └── loki.yaml
├── promtail/
│ ├── data/
│ └── promtail-config.yaml
├── alertmanager/
│ └── config.yml
├── blackbox/
│ └── config/ # Opcional: configuración avanzada
└── .env (opcional)

yaml
Copiar
Editar

---

## 🚀 Cómo levantar el stack

```bash
docker-compose up -d
Si deseas reiniciar todo borrando contenedores y volúmenes:

bash
Copiar
Editar
docker-compose down -v
⚠️ Importante: Los volúmenes están montados para persistencia real (dashboards, configuraciones, logs). Evita el uso de volúmenes anónimos si quieres conservar la configuración entre reinicios.

🌐 Accesos
Grafana: http://localhost:3260
Usuario: admin
Contraseña: admin

Prometheus: http://localhost:9090

Alertmanager: http://localhost:9093

Blackbox Exporter: http://localhost:9115

cAdvisor: http://localhost:8082

Node Exporter: http://localhost:9100

📊 Dashboards recomendados (ID de Grafana)
Puedes importarlos desde la opción “Import Dashboard” en Grafana:

Dashboard	ID	Fuente
Prometheus Stats	2	Grafana.com
Docker Containers (cAdvisor)	193	Grafana.com
Node Exporter Full	1860	Grafana.com
Loki Logs Overview	12019	Grafana.com
Blackbox Exporter Endpoint Monitoring	9964	Grafana.com

📦 Volúmenes persistentes
./grafana/data: Dashboards, datasources y credenciales.

./prometheus/prometheus.yml: Scrape configs.

./loki/loki.yaml: Configuración de almacenamiento de logs.

./promtail/promtail-config.yaml: Paths y targets de logs.

📌 Notas adicionales
Los errores de permisos (Permission denied) pueden solucionarse ejecutando:

bash
Copiar
Editar
sudo chown -R 472:472 grafana/data
(472 es el UID/GID por defecto usado por el contenedor de Grafana)

Se recomienda usar docker volume o bind mounts explícitos y controlados para evitar pérdida de información.

