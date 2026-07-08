# Home Assistant CLI Skill

An opencode-compatible `SKILL.md` for working with the Home Assistant CLI (`hass-cli`).

This skill follows the [Agent Skills](https://agentskills.io/) format.

The skill guides agents through safe Home Assistant command-line workflows: inspecting states, listing and calling services, working with devices and areas, watching events, rendering templates, and using Home Assistant Operating System supervisor/core commands.

## Available Skill

### hass-cli

Use when working with the Home Assistant CLI (`hass-cli`) to inspect, automate, or administer Home Assistant from the command line.

**Use when:**

- Inspecting Home Assistant states, services, devices, areas, or events
- Calling Home Assistant services through `hass-cli service call`
- Rendering Home Assistant templates from the command line
- Using `hass-cli ha ...` for Home Assistant Operating System supervisor/core commands
- Troubleshooting `hass-cli` setup, output formats, or command syntax

**What it covers:**

- `hass-cli` installation and setup checks
- `HASS_SERVER`, `HASS_TOKEN`, and `HASS_SUPERVISOR_TOKEN` handling
- Read-only discovery before state-changing commands
- State, service, area, device, event, template, raw API, and `ha` command groups
- Safer confirmation rules for changes and administrative operations
- JSON/YAML output recommendations for parseable results

## Install

### skills.sh

If your agent setup supports `skills`, install directly from GitHub:

```bash
npx skills add roman-pinchuk/hass-cli-skills
```

To install only this skill explicitly:

```bash
npx skills add roman-pinchuk/hass-cli-skills --skill hass-cli
```

### opencode

Clone this repository into an opencode skills directory:

```bash
git clone https://github.com/roman-pinchuk/hass-cli-skills.git ~/.config/opencode/skills/hass-cli-skills
```

Then restart opencode so it reloads available skills.

Project-local install:

```bash
mkdir -p .opencode/skills
git clone https://github.com/roman-pinchuk/hass-cli-skills.git .opencode/skills/hass-cli-skills
```

### Other SKILL.md-compatible agents

Clone or copy this folder into the skills directory watched by your agent runtime. The important file is [`SKILL.md`](./SKILL.md).

## Skill Structure

```text
SKILL.md        Instructions for the agent
README.md       Installation and usage notes
skills.sh.json  Registry/grouping metadata
```

## Requirements

This skill assumes the user may have `hass-cli` installed and configured. The CLI itself is provided by the upstream project:

<https://github.com/home-assistant-ecosystem/home-assistant-cli>

Common install methods include:

```bash
pip install homeassistant-cli
brew install homeassistant-cli
```

`hass-cli` normally connects through these environment variables:

```bash
export HASS_SERVER=http://homeassistant.local:8123
export HASS_TOKEN=<long-lived-access-token>
```

For Home Assistant Operating System supervisor commands, `HASS_SUPERVISOR_TOKEN` may also be required.

If Home Assistant is behind a reverse proxy with extra authentication, such as Pangolin, Cloudflared, or a similar access gateway, configure a bypass rule that allows unauthenticated access specifically for the `/api/*` and `/auth/*` paths. `hass-cli` still authenticates to Home Assistant with its Home Assistant token, but proxy-level auth on those paths can block CLI/API access.

## Safety Model

The skill tells agents to:

- Avoid printing access tokens or secrets
- Prefer read-only discovery commands first
- Ask before calling services, editing state, assigning devices, deleting areas, creating backups, updating Core, or using modifying raw API calls
- Verify changes with read-only follow-up commands

## License

MIT. See [`LICENSE`](./LICENSE).
