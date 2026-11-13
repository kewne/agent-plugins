# Agent Plugins Marketplace

A GitHub Copilot agent plugin marketplace containing plugins for software engineering workflows.

## Plugins

| Plugin | Description |
|---|---|
| [doks](./plugins/doks/) | Maintain high-quality technical documentation and specifications as the source of truth for spec-anchored development. |

## Install

```sh
copilot plugin marketplace add kewne/agent-plugins
copilot plugin marketplace browse agent-plugins
copilot plugin install doks@agent-plugins
```

## Try a local plugin

To test a plugin locally during development without installing it globally:

```sh
copilot --plugin-dir "/workspaces/agent-plugins/plugins/doks"
```

## License

This repository is licensed under the [GNU Affero General Public License v3.0](./LICENSE) (`AGPL-3.0-only`).
