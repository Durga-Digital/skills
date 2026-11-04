# Durga Digital Skills

Reusable agent skills for Durga Digital apps. Each skill teaches an agent a
specific workflow using the app's tools and APIs.

## Apps

| App | Skills | Status |
| --- | --- | --- |
| [Jessse](skills/jessse/README.md) | [Brand workflows](skills/jessse/jessse-brand/SKILL.md), [research](skills/jessse/jessse-research/SKILL.md) | Available |
| [Conclude](skills/conclude/README.md) | — | Scaffolded |
| [Jettworth](skills/jettworth/README.md) | — | Scaffolded |
| [Narada](skills/narada/README.md) | — | Scaffolded |

## Install

Use the [Skills CLI](https://github.com/vercel-labs/skills) with Node.js installed.
The CLI currently requires Node.js 22.20 or later. It supports Claude Code,
Codex and other compatible agents.

```bash
# Browse available skills without installing
npx skills add Durga-Digital/skills --list

# Choose skills and agents interactively
npx skills add Durga-Digital/skills

# Install just Jessse research to the current project
npx skills add Durga-Digital/skills --skill jessse-research

# Make Jessse research available across projects in Codex
npx skills add Durga-Digital/skills --skill jessse-research -a codex -g

# Or install it globally for Claude Code
npx skills add Durga-Digital/skills --skill jessse-research -a claude-code -g
```

Install both Jessse skills by repeating `--skill`:

```bash
npx skills add Durga-Digital/skills --skill jessse-brand --skill jessse-research
```

You can also ask a coding agent with installation access:

> Install the Jessse research skill from https://github.com/Durga-Digital/skills/tree/main/skills/jessse/jessse-research for this project.

A link can be read as instructions without installing it. Persistent installation
depends on the client; ordinary browser chat may not support local skill installation.

## Connect your app

Installing a skill supplies instructions. It does not sign in, connect an MCP
server, or grant access to your account. Connect the relevant app and approve the
required permissions separately. Each skill describes its prerequisites.

## Updates

```bash
npx skills check
npx skills update
```

The update command checks installed skills; installing one skill does not install
the rest of this catalog.

## Repository layout

```text
skills/
├── jessse/
│   ├── README.md
│   ├── jessse-brand/SKILL.md
│   └── jessse-research/SKILL.md
├── conclude/README.md
├── jettworth/README.md
└── narada/README.md
```

App folders group skills; each installable skill has its own `SKILL.md` and a
unique app-prefixed name. Optional `references/`, `scripts/` and `assets/` live
inside that skill's directory. Empty app scaffolds contain no placeholder
`SKILL.md` files, so installers only list usable skills.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). This repository is the shared source for
public customer skills. App-specific agent instructions, project templates and
software-factory tooling belong in their own repositories.

## License

[MIT](LICENSE) — Copyright (c) 2026 Durga Digital.
