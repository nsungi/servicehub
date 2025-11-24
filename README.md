## NNV Stack Overview

This repository is an **orchestration workspace** that brings together several upstream/open‑source projects into a single stack, managed via the root `docker-compose.yml`.

Each application lives in its **own vendor folder** (with its own internal structure, Docker files, and documentation). The root compose file is the **only place** where the combined architecture and ports are defined for your environment.

### Top‑Level Layout

- `docker-compose.yml` – main orchestration file for the NNV stack.
- `akto/` – full Akto API Security repository (uses its own Mongo/Kafka stack).
- `camel/` – full Camel AI / multi‑agent framework repository.
- `casibase/` – full CasiBase Go application (with its own `docker-compose.yml`).
- `entropy-data-ce/` – Entropy Data CE deployment templates (including its own `docker-compose.yaml`).
- `Flowise/` – full Flowise monorepo with its own `docker/` folder and compose files.
- `keycloak/` – full Keycloak source tree (you are using the official container image from `quay.io` in the root compose).
- `lobe-chat/` – full LobeChat monorepo (with its own Docker and self‑hosting docs).
- `nginx-ui/` – full nginx‑ui Go + Vue application (with demo Docker files).
- `trendradar/` – TrendRadar Python app with its own `docker/` folder and compose files.

These vendor folders are treated as **upstream source**:

- Their internal structure is intentionally left as‑is to stay compatible with their official docs.
- The root `docker-compose.yml` decides **which images** and **which configuration** are actually used in this NNV environment.

### How Deployment Works Here

- Use **only the root** `docker-compose.yml` to start the integrated stack:
  - It defines services such as `keycloak`, `flowise`, `nginx-ui`, `casibase`, `lobechat`, etc.
  - It wires services to a **shared external PostgreSQL instance** via environment variables (see comments inside the file).
- The individual vendor repositories may contain their own `docker-compose.yml` \- those are useful as references, but they are **not** what you run for the unified NNV deployment.

### Do Not Restructure Vendor Folders

To keep updates and documentation simple:

- Avoid moving/renaming folders under:
  - `akto/`, `camel/`, `casibase/`, `entropy-data-ce/`, `Flowise/`,
  - `keycloak/`, `lobe-chat/`, `nginx-ui/`, `trendradar/`.
- Follow each project’s own README for development details, but treat the root `docker-compose.yml` as the **single source of truth** for how everything runs together.

For a per‑service summary (images, ports, and source folders), see `services.md`.


#### docker exec -it your_postgres_container psql -U postgres -d postgres 





############################################################
# Shared PostgreSQL (ONE database for all services)
############################################################

# Where Postgres is reachable from the containers
POSTGRES_HOST=localhost
POSTGRES_PORT=5432

# Single Postgres user/password
POSTGRES_USER=coder
POSTGRES_PASSWORD=coder

# All services use the SAME database
POSTGRES_DB_KEYCLOAK=demo_service
POSTGRES_DB_FLOWISE=demo_service
POSTGRES_DB_CASIBASE=demo_service
POSTGRES_DB_LOBECHAT=demo_service
POSTGRES_DB_NGINX_UI=demo_service
POSTGRES_DB_ENTROPYDATA=demo_service


############################################################
# Keycloak
############################################################

# Host port → container 8080
KEYCLOAK_HTTP_PORT=8180

# Initial admin (you can change these)
KEYCLOAK_ADMIN_USER=admin
KEYCLOAK_ADMIN_PASSWORD=change-me


############################################################
# Flowise
############################################################

# Host port → container 3000
FLOWISE_HTTP_PORT=3000

# Public URL for Flowise (used in links/callbacks)
APP_URL=http://localhost:${FLOWISE_HTTP_PORT}


############################################################
# nginx-ui
############################################################

TZ=UTC

# Host ports → container 80 / 443
NGINX_UI_HTTP_PORT=9000
NGINX_UI_HTTPS_PORT=9443


############################################################
# CasiBase
############################################################

# Host port → container 14000
CASIBASE_HTTP_PORT=14000


############################################################
# LobeChat
############################################################

# Host port → container 3210
LOBECHAT_HTTP_PORT=3210


############################################################
# Entropy Data CE
############################################################

# Host port → container 8081
ENTROPYDATA_HTTP_PORT=8081

# Public URL used in Entropy Data emails
ENTROPYDATA_HOST_WEB=http://localhost:${ENTROPYDATA_HTTP_PORT}

# From address for Entropy Data emails
ENTROPYDATA_MAIL_FROM="Entropy Data <noreply@example.com>"

# SMTP settings (replace with real SMTP values)
ENTROPYDATA_SMTP_HOST=smtp.example.com
ENTROPYDATA_SMTP_PORT=587
ENTROPYDATA_SMTP_USER=your_smtp_user
ENTROPYDATA_SMTP_PASSWORD=your_smtp_password
ENTROPYDATA_SMTP_AUTH=true
ENTROPYDATA_SMTP_STARTTLS=true


############################################################
# Akto (Mongo-based, alongside Postgres)
############################################################

# Dashboard host port → container 8080
AKTO_DASHBOARD_HTTP_PORT=9090

# Puppeteer replay host port → container 3000
AKTO_PUPPETEER_HTTP_PORT=3001