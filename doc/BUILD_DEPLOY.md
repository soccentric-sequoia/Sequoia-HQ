# Sequoia-HQ Build and Deployment

## Scope

Sequoia-HQ is the local multi-service control plane: Python API and agent
gateway, Vue frontend, and Docker Compose dependencies (Postgres, MinIO, Redis,
and Keycloak). The Makefile owns the virtualenv, npm install/build, migrations,
seed data, background service lifecycle, tests, and linting.

## 1. Prerequisites

`make doctor` checks Docker, Python 3, and Node. Docker Compose must be
available as `docker compose`; network access is needed to pull images and
Python/npm dependencies.

```sh
make doctor
make install                  # .venv, editable api/gateway dev deps, web npm deps
```

The default development database URLs, MinIO endpoint, credentials, ports, and
Keycloak realm are defined in the Makefile and `infra/docker-compose.yml`.

## 2. Setup

## 3. Build

```sh
make build                    # web production build via npm run build
```

The API and gateway are Python services and do not have a separate compile
target; editable installation prepares them in `.venv`.

## 4. Test and lint

```sh
make test                     # API, gateway, and web Vitest suites
make test-api
make test-gateway
make test-web
make lint                     # ruff plus web ESLint and vue-tsc
make lint-api
make lint-gateway
make lint-web
```

Tests require the installed dependencies and the test database/object-store
services started by `make db-up`.

## 5. Artifacts

No package/image target exists in this root Makefile. The frontend build is in
`web/dist`; runtime state and logs are in `.run`. The Compose file supplies
development service images and persistent Docker volumes, not a production
release artifact.

## 6. Deploy, install, and run

```sh
make db-up                    # start dependencies and wait for readiness
make migrate                  # alembic upgrade head
make seed EMAIL=admin@example.com  # explicit dev admin seed
make run                      # start API, gateway, and web in background
make logs
make stop                     # stop app processes; DB remains running
make db-down                  # stop Compose dependencies
```

`make run` reports LAN URLs: web `5173`, API `8000`, Keycloak `8081`, and
gateway `50051`. The development Keycloak login is `admin/admin`; the seeded
application user is `admin@example.com` / `devpassword`. **Local development
credentials only; never expose this stack outside a trusted network.**

## 7. Cleanup

```sh
make clean                    # remove venv, node_modules, caches, dist, logs
make db-down                  # stop containers, retaining named volumes
```

## 8. Verification caveats

This Makefile defines a local Compose deployment, not cloud provisioning or a
device deployment. Manually verify OIDC redirects using the reported LAN URL,
database migrations, MinIO object flows, gateway connectivity, and integration
with real Sequoia devices. Persistent Docker volumes retain data after
`db-down`; remove them deliberately when a clean environment is required.