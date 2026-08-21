# Sequoia-HQ

## Project Summary

Sequoia-HQ is the planned cloud fleet-management surface for Sequoia devices. The repository-level
documentation describes remote device viewing, pairing and authorization, telemetry, faults and
alarms, logs, OTA updates, backup/restore, provisioning, factory reset, disable, and location
workflows. The checked-in top-level README labels the project `planning`; the available source and
configuration tree should therefore be read as an evolving product surface rather than a production
readiness claim. Supporting documentation includes a changelog, agent/project guidance, Alembic
configuration, and a web package README, while the repository also contains service/configuration
code and tests in its subdirectories.

This README keeps the useful product scope but distinguishes it from verified behavior. Pairing,
RBAC, OIDC, tenant isolation, device and telemetry routes, alarms, agent-gateway frames,
stale-session sweeping, and selected web components are represented in the test inventory. The
presence of those tests is useful evidence of intended contracts, not proof of a deployed cloud
environment, external identity provider, real device fleet, or migration rollout. Database,
infrastructure, authentication secrets, and networked integration require environment-specific
checks. Exact commands should be taken from the root and service Makefiles and the relevant
Python/TypeScript package manifests.

The planning status is particularly relevant for the fleet operations listed in the original README:
a named workflow is not proof that a complete route, worker, authorization rule, migration, or
device-side action exists. The inventory records the strongest evidence visible in source and tests
while leaving broad operations partial or planned until their end-to-end boundaries are
demonstrated.

## Feature Inventory

| # | Name / Description | Implemented? | Test Coverage? | Manual Test |
| --- | --- | --- | --- | --- |
|   1 | Fleet-management product boundary and service layout | Partial      | Service tests present                                     | [ ] Manual verification required |
|   2 | Device pairing and authorization                     | Partial      | `api/tests/test_pairing_tokens.py`, gateway pairing tests | [ ] Manual verification required |
|   3 | Role-based access control                            | Partial      | `api/tests/test_rbac.py`                                  | [ ] Manual verification required |
|   4 | OIDC authentication integration                      | Partial      | `api/tests/test_oidc.py`                                  | [ ] Manual verification required |
|   5 | Tenant isolation                                     | Partial      | `api/tests/test_tenant_isolation.py`                      | [ ] Manual verification required |
|   6 | Device and telemetry APIs                            | Partial      | `test_devices.py`, `test_telemetry_router.py`             | [ ] Manual verification required |
|   7 | Faults, alarms, and web alarm panel                  | Partial      | Alarm API and web component tests                         | [ ] Manual verification required |
|   8 | Agent-gateway frames and stale sweep                 | Partial      | Gateway frame/stale tests                                 | [ ] Manual verification required |
|   9 | OTA, backup/restore, provisioning, and factory reset | Planned      | No focused evidence identified                            | [ ] Manual verification required |
|  10 | Web operator views and shared utilities              | Partial      | Vue component/store tests present                         | [ ] Manual verification required |
|  11 | Alembic/database migration path                      | Unknown      | Migration integration coverage requires environment       | [ ] Manual verification required |
|  12 | Production fleet deployment                          | Planned      | No deployment verification found                          | [ ] Manual verification required |

## Validation

Use the root `Makefile` and the service package manifests to select the Python and web checks.
Relevant test directories are `api/tests/`, `agent-gateway/tests/`, and web `__tests__` directories.
A live identity provider, database, and device gateway are required for manual integration.
