## Services Map

This file summarizes how the **root** `docker-compose.yml` relates to the upstream projects checked into this workspace.

### Orchestrated Services (root `docker-compose.yml`)

| Service name | Docker image                          | Default host port(s)                      | Upstream source folder | Notes |
|-------------|----------------------------------------|-------------------------------------------|------------------------|-------|
| `keycloak`  | `quay.io/keycloak/keycloak:24.0`      | `${KEYCLOAK_HTTP_PORT:-8180}` → `8080`    | `keycloak/`            | Defaults to dev‑file DB; can be switched to PostgreSQL via `KC_DB=postgres` and JDBC env vars. |
| `flowise`   | `flowiseai/flowise:latest`            | `${PORT:-3000}` → same inside container   | `Flowise/`             | Defaults to sqlite for demo; supports PostgreSQL via `DATABASE_TYPE=postgres` and `DATABASE_*` env vars. |
| `nginx-ui`  | `uozi/nginx-ui:latest`                | `${NGINX_UI_HTTP_PORT:-9000}` → `80`, `${NGINX_UI_HTTPS_PORT:-9443}` → `443` | `nginx-ui/`            | Manages nginx configs, stores its own data; does not use PostgreSQL. |
| `casibase`  | `casbin/casibase:latest`              | `${CASIBASE_HTTP_PORT:-14000}` → `14000`  | `casibase/`            | Uses PostgreSQL via `driverName=postgres` and `dataSourceName` DSN built from shared `POSTGRES_*` env vars. |
| `lobechat`  | `lobehub/lobe-chat:latest`            | `${LOBECHAT_HTTP_PORT:-3210}` → `3210`    | `lobe-chat/`           | Uses PostgreSQL via `DATABASE_URL` built from shared `POSTGRES_*` env vars. |

> **Note:** The actual Postgres connection details (host, port, user, password, DB names) are provided via environment variables or a `.env` file alongside `docker-compose.yml`. The comments in `docker-compose.yml` describe exactly which variables to set.

### Vendor Projects Present but Not Orchestrated Directly

These folders contain full upstream projects. They may have their own Docker and deployment setups, but they are **not** currently wired directly into the root `docker-compose.yml`:

- `akto/akto` – Akto API Security platform (uses MongoDB and other components in its own `docker-compose.yml`).
- `camel/camel` – Camel AI / multi‑agent framework (Python library + examples; typically used as a dependency, not via a single Docker service).
- `entropy-data-ce/entropy-data-ce` – Entropy Data CE deployment templates (`docker-compose.yaml`, cloud templates).
- `trendradar/trendradar` – TrendRadar Python app with `docker/` folder and its own compose.

If you decide to integrate any of these into the unified stack:

- Prefer using the **official images and env vars** from their upstream docs.
- Add a new service block to the root `docker-compose.yml` and map it to a free host port.
- Keep the vendor folders’ internal structure unchanged so that upstream updates and documentation remain valid.


