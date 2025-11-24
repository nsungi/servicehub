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

 
 