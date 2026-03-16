<p align="center">
  <h1 align="center">Zabbix Templates — ITS Connect</h1>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Zabbix-7.2-red?logo=zabbix&logoColor=white" alt="Zabbix 7.2">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License MIT">
  <img src="https://img.shields.io/badge/Agent-Zabbix%20Agent%202-orange" alt="Zabbix Agent 2">
</p>

<p align="center">
  Professional collection of Zabbix monitoring templates for Linux servers, Docker, MySQL, Nginx, Redis and SSL certificates.<br>
  Maintained by <a href="https://itsconnect.com.br">ITS Connect</a>.
</p>

---

## Templates

| Template | File | Description |
|----------|------|-------------|
| Linux Server | [linux-server.yaml](templates/linux-server.yaml) | CPU, RAM, disk, network, uptime and load average monitoring |
| Docker Containers | [docker-containers.yaml](templates/docker-containers.yaml) | Container status, resource usage and inventory |
| MySQL 8 | [mysql-monitoring.yaml](templates/mysql-monitoring.yaml) | Query throughput, connections, buffer pool and replication |
| Nginx | [nginx-status.yaml](templates/nginx-status.yaml) | Active connections, request rate and worker status |
| SSL Certificates | [ssl-certificates.yaml](templates/ssl-certificates.yaml) | Certificate expiry, issuer and validity monitoring |

## Requirements

- **Zabbix Server** 7.0 or later (tested on 7.2)
- **Zabbix Agent 2** installed on monitored hosts
- For MySQL monitoring: a dedicated zbx_monitor database user with read-only privileges
- For Nginx monitoring: stub_status module enabled on a local endpoint
- For Docker monitoring: Zabbix Agent 2 user added to the docker group

## Installation

1. Download the desired template YAML files from the [templates/](templates/) directory.
2. Open your Zabbix web interface.
3. Navigate to **Data collection** > **Templates**.
4. Click **Import** (top-right corner).
5. Select the YAML file and click **Import**.
6. Link the imported template to the desired hosts.

### Quick import via Zabbix API

```python
from zabbix_utils import ZabbixAPI

zapi = ZabbixAPI(url="https://zabbix.example.com")
zapi.login(token="your-api-token")

with open("templates/linux-server.yaml", "r") as f:
    zapi.configuration.import_({
        "format": "yaml",
        "source": f.read(),
        "rules": {
            "templates": {"createMissing": True, "updateExisting": True},
            "items": {"createMissing": True, "updateExisting": True},
            "triggers": {"createMissing": True, "updateExisting": True},
            "discoveryRules": {"createMissing": True, "updateExisting": True},
        }
    })
```

## Template Details

### Linux Server

Monitors core operating system metrics using Zabbix Agent 2 built-in keys. Includes triggers for critical thresholds on CPU, memory and disk usage, plus a notification when the server is restarted.

### Docker Containers

Uses Zabbix Agent 2 native Docker plugin to discover and monitor running containers. Tracks per-container CPU and memory consumption with automatic low-level discovery.

### MySQL 8

Monitors MySQL performance counters through the Zabbix Agent 2 MySQL plugin. Covers query throughput, connection pool saturation, InnoDB buffer pool efficiency and replication health.

### Nginx

Reads metrics from the Nginx stub_status endpoint. Monitors active connections, request throughput and the distribution of reading, writing and waiting workers.

### SSL Certificates

Performs periodic TLS handshakes to check certificate validity. Provides graduated alerts at 30 days (warning), 7 days (high) and expiry (disaster).

## Contributing

Contributions are welcome. Please open an issue or submit a pull request with your improvements.

## License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">
  <strong><a href="https://itsconnect.com.br">ITS Connect</a></strong> — Hospedagem e Infraestrutura de TI
</p>
