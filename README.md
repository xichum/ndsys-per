# NetCore Service Container

A lightweight, automated container for network stream forwarding and edge connectivity management. Designed for high-performance minimalist environments.

## Quick Start

### Docker CLI

```bash
docker run -d \
  --name netcore \
  --restart always \
  --network host \
  -v $(pwd)/data:/data \
  -e TZ="Asia/Shanghai" \
  -e H_PORT=20343 \
  -e R_PORT=20343 \
  ghcr.io/YOUR_USERNAME/YOUR_REPO:latest
```

### Docker Compose

```yaml
version: '3.8'
services:
  core:
    image: ghcr.io/YOUR_USERNAME/YOUR_REPO:latest
    restart: always
    network_mode: "host"
    volumes:
      - ./data:/data
    environment:
      - H_PORT=20343
      - R_PORT=20343
      - TZ=Asia/Shanghai
```

## Configuration

The service is controlled entirely via Environment Variables.

### Network Modules

| Variable | Default | Description |
| :--- | :--- | :--- |
| `T_PORT` | *None* | Port for Module T (TCP/UDP). Leave empty to disable. |
| `H_PORT` | `20343` | Port for Module H (UDP-based). |
| `R_PORT` | `20343` | Port for Module R (TCP-based). |

### System Settings

| Variable | Default | Description |
| :--- | :--- | :--- |
| `BIN_V` | *Latest* | Force a specific core binary version (e.g., `1.10.0`). |
| `ID_PRE` | `Srv` | Prefix tag for the generated link identifiers. |
| `LOG_ON` | `false` | Enable internal verbose logging (`true`/`false`). |
| `TZ` | *UTC* | System timezone (e.g., `Asia/Shanghai`). |

### Remote Resources

Auto-fetch external security certificates or keys on startup.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `REM_ON` | `true` | Enable remote resource fetching. |
| `REM_C` | *Default URL* | URL to fetch the public certificate (PEM). |
| `REM_K` | *Default URL* | URL to fetch the private key (PEM). |

## Persistence

It is highly recommended to mount the `/data` volume.
This directory stores:
*   Unique Instance UUIDs
*   Generated Keys
*   Cached Binaries
*   Runtime Logs

```bash
-v /host/path:/data
```

## Access Info

Upon startup, the container generates an initialization token.
View it via container logs:

```bash
docker logs netcore
```
