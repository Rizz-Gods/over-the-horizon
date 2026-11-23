# Configuration

Runtime defaults are stored as JSON under `config/`. The checked-in files use machine-neutral executable names and relative runtime paths. Adapt them to your environment before enabling optional workers.

## Machine-specific values

- `config/agents.json` — agent commands and browser adapter settings.
- `config/browser.json` — Chrome executable and local browser-profile directory.
- `config/tools.json` — commands for Git, Python, Node.js, FFmpeg, Git LFS, Hermes, and Agent Browser.
- `config/media.json` — FFmpeg and FFprobe commands.
- `config/production.json` — DaVinci Resolve executable candidates and media defaults.
- `config/model_routing.json` — local model provider and model names.
- `config/policies.json` and `config/workforce.json` — risk, permissions, and worker capability defaults.

Use executable names only when the executable is available on `PATH`; otherwise configure its full path locally. Keep personal overrides out of commits. Prefer environment variables or a separate local configuration file for secrets.

## Credentials

Do not store API keys, OAuth refresh tokens, passwords, or browser cookies in these JSON files. The current credential store uses Windows DPAPI to bind encrypted token material to the local Windows user. Copying the encrypted store to another account or machine is not a supported migration path.

## External integrations

External providers and desktop programs are optional. Install each dependency independently, check its terms and access requirements, and test it with disposable accounts or data. A configured command does not imply that the external service is authenticated or that the integration is approved for use.
