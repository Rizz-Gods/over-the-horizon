# Over The Horizon (OTH)

**A local-first, policy-gated Python runtime for coordinating agents, tasks, and tools.**

Over The Horizon places a task kernel between agent adapters and the tools they can invoke. The runtime models work as persisted tasks, applies risk-aware permission checks, routes execution to interchangeable workers, and records mission state so operators can inspect and recover work.

> **Project status: experimental.** OTH is a developing research and engineering codebase. It is not security-certified, production-hardened, or suitable for unattended high-impact operations.

## Core capabilities

- **Task kernel and persistence** — SQLite-backed task lifecycle, dispatch, retry, and scheduling primitives.
- **Mission control** — task graphs, progress state, audit records, continuity, replay, cancellation, and recovery helpers.
- **Policy gates** — risk-aware execution permissions, with external and financial actions configured to require approval.
- **Worker adapters** — built-in, browser, and external-agent execution interfaces.
- **Agent and workforce registries** — declarative capability, lane, tool, and workforce configuration.
- **Routing and fallbacks** — configurable model/execution lanes and fallback workers.
- **Operator surfaces** — a Python CLI and local console.
- **Optional integrations** — browser automation, local model tooling, media processing, and social workflow modules. These require separately installed tools, provider access, and explicit configuration.

## Requirements and platform notes

- Python 3.11 or newer
- Git
- Windows is the currently supported runtime platform for the integrated application.

The secure credential store currently uses Windows DPAPI. The core source may be useful for inspection on other platforms, but the full runtime is not currently portable to Linux or macOS. Optional modules may also require external programs such as Ollama, a browser automation executable, FFmpeg, or DaVinci Resolve. These are not bundled with OTH.

## Install

Clone the repository and install it in an isolated environment:

```powershell
git clone https://github.com/Rizz-Gods/over-the-horizon.git
cd over-the-horizon

py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1

python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

Some optional media features have large dependencies. Install only the extras you need; review `pyproject.toml` before installing optional GPU or desktop dependencies.

## Run

Show the available commands:

```powershell
oth --help
```

Run the test suite:

```powershell
python -m pytest
```

The default runtime configuration lives in `config/`. Review it before running any worker or connecting an external provider. Replace example executable names with commands available on your own machine. Do not put credentials in JSON configuration.

## Repository structure

```text
oth/                    Runtime package
  core/                 Kernel, state, policy, routing, mission and service modules
  workers/              Built-in, browser and external worker adapters
config/                 Public, machine-neutral runtime configuration
docs/                   Engineering and mission-control contracts
tests/                  Automated tests
.github/workflows/      Continuous integration
```

## Security and safe operation

The policy layer is a control boundary, not a guarantee that every integration is safe. Review the policy configuration and the code path for each worker before enabling it.

- Begin with disposable data and non-destructive tasks.
- Use least-privilege credentials and keep secrets in the OS-backed credential store or a dedicated secret manager.
- Require human approval for external, financial, irreversible, or otherwise high-impact actions.
- Treat model and agent output as untrusted input.
- Never commit access tokens, OAuth material, passwords, private databases, operational logs, browser profiles, or machine-specific paths.
- Do not use this project for unattended financial activity, safety-critical systems, or production automation without independent threat modelling and validation.

See [SECURITY.md](SECURITY.md) for reporting guidance and [docs/](docs/) for design contracts.

## Contributing

Focused bug reports, tests, documentation improvements, and pull requests are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) and follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## License

Released under the [MIT License](LICENSE).
