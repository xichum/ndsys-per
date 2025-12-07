# Network Stream Controller (NSC)

A containerized solution for high-performance stream routing and edge endpoint management. Designed for deployment in minimal environments.

## Quick Start

**Docker CLI**

```bash
docker run -d \
  --name nsc-node \
  --restart always \
  --network host \
  -v $(pwd)/data:/data \
  -e TZ="Asia/Shanghai" \
  -e H_PORT=51130 \
  -e T_PORT=51131 \
  -e R_PORT=51132 \
  -e S_ID="your-custom-uuid-here" \
  ghcr.io/YOURNAME/REPO:latest
```

## Configuration Reference

All configurations are managed via Environment Variables.

### 1. Network Endpoints
Define the listening ports for different transport protocols.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `T_PORT` | *Disabled* | **Transport-T** listening port (TCP/UDP). |
| `H_PORT` | `20343` | **Transport-H** listening port (UDP). |
| `R_PORT` | `20343` | **Transport-R** listening port (TCP). |

### 2. Session & Authentication
Manage identity and access control.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `S_ID` | *Auto-Gen* | **Master Session ID**. If set, this static ID will persist across restarts. |
| `T_PASS` | `admin` | Access token for Transport-T. |
| `H_PASS` | *Same as S_ID* | Access token for Transport-H. |

### 3. Upstream Routing
Configuration for Transport-R upstream handshake (Handshake/Dest).

| Variable | Default | Description |
| :--- | :--- | :--- |
| `R_DOM` | `www.nazhumi.com` | **Target Hostname**. The domain used for handshake verification. |
| `R_DOM_P`| `443` | **Target Port**. The port of the target host. |

### 4. System & Logging

| Variable | Default | Description |
| :--- | :--- | :--- |
| `ID_PRE` | `Srv` | **Node Label**. Prefix used in the exported config string. |
| `BIN_V` | *Latest* | **Core Version**. Pin a specific binary version (e.g., `1.10.0`). |
| `LOG_ON` | `false` | **Debug Mode**. Set to `true` to enable verbose file logging. |
| `TZ` | *UTC* | **Timezone**. System timezone (e.g., `Asia/Shanghai`). |

### 5. Remote Resources
Auto-provisioning of security assets (Certificates/Keys) from external URLs.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `REM_ON` | `true` | Enable remote asset fetching. |
| `REM_C` | *Default* | URL for the **Public Asset** (PEM format). |
| `REM_K` | *Default* | URL for the **Private Asset** (Key format). |

## Data Persistence

Mounting `/data` is recommended to persist the Session ID and cached binaries.

```bash
-v /your/local/path:/data
```

## Status Output

To retrieve the initialization string (Base64 encoded configuration):

```bash
docker logs nsc-node
```
