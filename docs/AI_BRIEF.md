# ATLAS Appliance — AI_BRIEF (Source of Truth)

## 1. Purpose (Business)
ATLAS Appliance is the customer-facing self-hosted distribution of ATLAS.
It must provide a simple, reliable “appliance-like” install and upgrade experience
with minimal support burden.

Primary goal:
- One-command install
- Stable runtime
- Clean docs and troubleshooting
- Runs behind customer Traefik (reverse proxy)

## 2. Scope (What this repo contains)
- Docker Compose stack and service wiring
- Install/upgrade/uninstall scripts
- Docs for customers (Quickstart / Troubleshooting)
- Bundled n8n workflow templates (as files)
- Environment templates (.env.example)
- Minimal health checks and diagnostics

Out of scope:
- Core scoring logic (belongs to atlas-core)
- Licensing backend (belongs to atlas-licensing)
- Secrets/key generation embedded in the repo

## 3. EPIC Alignment
EPIC 01 Deployment & Distribution:
- install.sh / upgrade.sh / uninstall.sh
- docker-compose.yml
- docs/quickstart.md, docs/troubleshooting.md

EPIC 06 UX:
- clear errors, guided steps, “what to do next”
- minimal UI/route setup (Traefik labels) for service access

EPIC 05 Security:
- no secrets in git
- least privilege runtime
- safe defaults (no public ports unless needed)

EPIC 04 Automation:
- ships n8n templates; does not hardcode customer credentials

EPIC 08 Observability:
- health endpoints + logs + basic diagnostics

## 4. Runtime Architecture (Initial)
Services (compose):
- atlas-core (container, API)
- n8n (container)
- postgres (for n8n) recommended
Optional (later):
- redis, ui, exporters

Networking:
- Uses external Traefik network: `traefik` (external: true)
- Creates internal network: `atlas-internal`
Exposure:
- No direct published ports by default
- Traefik labels define routers/services (hostnames)

Persistence:
- Named volumes for n8n + postgres
- Compose must support upgrades without data loss

## 5. Configuration & Secrets
- Provide `.env.example` only
- Real values provided at deploy time (`.env` ignored by git)
- No secret values in docker-compose.yml
- Support: DOMAIN names, basic credentials, DB creds, tokens as env vars

## 6. Security Requirements
- No secrets committed (scan-friendly)
- Healthchecks must not leak sensitive data
- Hardening defaults (read-only filesystem where possible, drop caps where possible)
- Clear guidance: how to rotate secrets, how to back up volumes

## 7. Docs Requirements (Support minimization)
Required docs:
- docs/quickstart.md (install + first run)
- docs/upgrade.md (safe upgrade + rollback basics)
- docs/troubleshooting.md (top 10 issues)
- docs/architecture.md (high-level)

Docs style:
- step-by-step
- copy/paste friendly
- explicit file paths, commands, expected outputs

## 8. File Expectations
Must exist:
- docker-compose.yml
- install.sh
- docs/AI_BRIEF.md (this file)
- .env.example
- docs/quickstart.md
- docs/troubleshooting.md
- n8n/templates/ (workflow templates)

## 9. Definition of Done
- `docker compose up` starts stack consistently
- `install.sh` is idempotent + clear errors
- Traefik labels route to atlas-core and n8n (no direct ports)
- Backup/restore documented
- Upgrade path documented
