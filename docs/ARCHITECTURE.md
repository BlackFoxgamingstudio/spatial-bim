# Architecture: Sovereign Spatial BIM

## Overview

**Package ID:** `PKG-002`  
**Domain:** 3D Graphics & Spatial BIM  
**Microservice Port:** `8780`  
**n8n Webhook Path:** `spatial-bim-trigger`  
**GitHub:** [BlackFoxgamingstudio/spatial-bim](https://github.com/BlackFoxgamingstudio/spatial-bim)

Building Information Modeling engine with IFC parsing, clash detection, 3D mesh optimization, spatial queries, and AR/VR export for field inspection workflows.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign Spatial BIM         │
                     │       Port: 8780            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  IFCParser       | ClashDetector   | MeshOptimize  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `IFCParser`
Handles all ifcparser operations. Exposes async methods callable from the core dispatcher.

### `ClashDetector`
Handles all clashdetector operations. Exposes async methods callable from the core dispatcher.

### `MeshOptimizer`
Handles all meshoptimizer operations. Exposes async methods callable from the core dispatcher.

### `SpatialQueryEngine`
Handles all spatialquery operations. Exposes async methods callable from the core dispatcher.

### `ARExporter`
Handles all arexporter operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-spatial-bim", "port": 8780}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-spatial-bim:
  image: sovereign-spatial-bim:latest
  ports: ["8780:8780"]
  healthcheck:
    test: curl -f http://localhost:8780/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`bim`, `ifc`, `3d`, `spatial`, `construction`
