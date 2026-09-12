# Changelog — Sovereign Spatial BIM

All notable changes follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.
Versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Full ecosystem documentation suite (ARCHITECTURE, DEVELOPER_GUIDE, SME_PLAYBOOK, SOP)
- GitHub Actions CI matrix (Python 3.10 / 3.11 / 3.12)
- Multistage Dockerfile with non-root user, health check, OCI labels
- docker-compose.yml with SBB platform network
- n8n custom node integration via `SovereignTools`
- `.env.example` environment template
- CONTRIBUTING, CODE_OF_CONDUCT, SECURITY governance files
- OpenAPI 3.1 compatible REST API spec
- Bandit security scan in CI

## [1.0.0] — 2024-01-01

### Added
- Initial production release of Sovereign Spatial BIM (PKG-002)
- Core microservice on port `8780`
- n8n webhook adapter (`n8n/webhook_adapter.py`)
- REST API (`POST /api/v1/execute`, `GET /health`)
- Components: IFCParser, ClashDetector, MeshOptimizer, SpatialQueryEngine, ARExporter
- pyproject.toml packaging with `[dev]` extras
- CLI: `sovereign-spatial-bim --help`
- Unit test suite (pure `unittest.TestCase`, no external test framework required)

### Domain: 3D Graphics & Spatial BIM
Building Information Modeling engine with IFC parsing, clash detection, 3D mesh optimization, spatial queries, and AR/VR export for field inspection workflows.

[Unreleased]: https://github.com/BlackFoxgamingstudio/spatial-bim/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BlackFoxgamingstudio/spatial-bim/releases/tag/v1.0.0
