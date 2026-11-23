# Architecture

Over The Horizon (OTH) is a local-first runtime. Its main boundary is the task kernel: callers submit work to the kernel, the kernel stores execution state, applies policy checks, selects a worker, and records the outcome.

## Execution path

1. **Submission** — the CLI or another local caller describes a capability, action, payload, and risk level.
2. **Persistence** — task and mission stores write state to SQLite beneath the local runtime data directory.
3. **Policy evaluation** — the policy gate checks the requested operation against its risk and permission requirements. Operations that require approval should remain blocked until an operator approves them.
4. **Routing** — registries and lane configuration select a suitable worker or fallback.
5. **Execution** — built-in, browser, or external adapters perform the operation. Integrations may invoke local programs or external providers.
6. **Evaluation and audit** — results are normalized, evaluated, and persisted with mission progress and audit information.
7. **Recovery** — retry, replay, cancellation, continuity, and mission-state helpers support operator-led recovery.

## Main packages

- `oth/core/kernel.py` — runtime composition and task orchestration.
- `oth/core/db.py` and `oth/core/mission_state.py` — SQLite-backed task and mission persistence.
- `oth/core/policy.py` — risk and approval checks.
- `oth/core/registry.py`, `workforce.py`, `tools.py`, and `lanes.py` — capability, worker, tool, and execution-lane registration.
- `oth/workers/` — built-in, browser, and external worker adapters.
- `oth/cli.py` — command-line entry point.
- `oth/console.py` and `console/index.html` — local operator console.
- `config/` — runtime behaviour and integration configuration.
- `tests/` — automated unit and contract tests.

## Trust boundaries

Agent and model output is untrusted input. External commands, browser sessions, provider APIs, local files, and public publishing destinations are separate trust boundaries. The policy layer is intended to gate consequential operations, but every adapter still needs its own review, tests, and least-privilege configuration.

The current credential store uses Windows DPAPI. The integrated runtime is therefore Windows-first; this is not yet a cross-platform secure-storage abstraction.

## State and data

The application creates its databases, browser profiles, logs, and encrypted credential state locally under `data/`. This directory is intentionally excluded from version control. Do not commit production databases, browser profiles, token stores, generated media, or operational logs.
