# Observability

Shared local observability infrastructure for agentic projects using OpenTelemetry.
See the [observability-hooks repo](https://github.com/bbenedict/observability-hooks) for how to integrate this infrastructure into your Claude Code projects.

The project runs entirely in Docker and provides:

- **OpenTelemetry Collector** — receives agentic telemetry from projects
- **Prometheus** — stores and queries metrics
- **Tempo** — stores and queries traces
- **Grafana** — visualizes metrics and traces

## Prerequisite

The only prerequisite is **Docker** with Docker Compose support.

### Docker for macOS

1. Download [Docker Desktop for Mac](https://docs.docker.com/desktop/setup/install/mac-install/).
2. Choose the correct version for **Apple silicon** or **Intel**.
3. Open the downloaded `.dmg` and drag Docker into **Applications**.
4. Launch **Docker Desktop** and complete the setup.
5. Verify:

    ```bash
    docker --version
    docker compose version
    ```

### Docker Windows

1. Download [Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/).
2. Run the installer.
3. Use the **WSL 2** backend when prompted.
4. Restart Windows if requested, then launch **Docker Desktop**.
5. Verify in PowerShell:

    ```powershell
    docker --version
    docker compose version
    ```

## First-Time Setup

From the project directory, create and start all containers:

```bash
docker compose up -d
```

Check that the services are running:

```bash
docker compose ps
```

Once started, the following services are available locally:

| Service | Address |
| --- | --- |
| Grafana | http://localhost:3000 |
| Prometheus | http://localhost:9090 |
| Tempo | http://localhost:3200 |
| OTLP/gRPC | localhost:4317 |
| OTLP/HTTP | localhost:4318 |

Grafana is automatically configured with Prometheus and Tempo as data sources.

Login in with the default username and password. You'll be asked yo change on first login:

```
username: admin
password: admin
```

### Persistent Data

Prometheus, Tempo, and Grafana store their data in Docker named volumes:

- `prometheus-data`
- `tempo-data`
- `grafana-data`

These volumes are created automatically during setup. They preserve metrics, traces, Grafana dashboards, and other Grafana data when the containers are stopped or recreated.

## Normal Usage

After the initial setup, use `start` and `stop` for normal operation.

### Start

```bash
docker compose start
```

### Stop

```bash
docker compose stop
```

Stopping the services this way preserves the containers and all persistent data.


## Rebuilding the Containers

If `docker-compose.yml` changes or you need to recreate the containers:

```bash
docker compose down
docker compose up -d
```

`docker compose down` removes the containers and Docker network, but **does not remove the persistent data volumes**. Your Prometheus, Tempo, and Grafana data will remain intact.
