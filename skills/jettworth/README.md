# JettWorth skills

Use your agent to understand your finances, compare possibilities and build
scenarios through JettWorth's MCP tools.

| Skill | When to use it | Access |
| --- | --- | --- |
| [jettworth-financial-review](jettworth-financial-review/SKILL.md) | A financial check-in: explain net worth, history, income, expenses and plan assumptions. | Read |
| [jettworth-plan-comparison](jettworth-plan-comparison/SKILL.md) | Compare saved plans fairly over the same horizon and explain the tradeoffs. | Read |
| [jettworth-stress-test](jettworth-stress-test/SKILL.md) | Explore lower growth or higher inflation with temporary assumptions, without saving. | Read |
| [jettworth-scenario-planning](jettworth-scenario-planning/SKILL.md) | Save or edit a scenario for a career break, salary change or retirement age, then compare it. | Read and scenario editing |

Each skill is independently installable. Start with financial review; add the
others for the workflows you use. All require JettWorth Pro and an approved MCP
connection. Scenario editing also requires an existing base plan and explicit
editing consent, and is available only where the connection exposes write tools.

## Install

With Node.js 22.20 or later, use the [Skills CLI](https://github.com/vercel-labs/skills):

```bash
# Browse the catalog without installing
npx skills add Durga-Digital/skills --list

# Install the financial review skill into the current project
npx skills add Durga-Digital/skills --skill jettworth-financial-review

# Install all four JettWorth skills; choose your agent interactively
npx skills add Durga-Digital/skills \
  --skill jettworth-financial-review \
  --skill jettworth-plan-comparison \
  --skill jettworth-stress-test \
  --skill jettworth-scenario-planning
```

For availability across projects, add `-g`. To choose a client explicitly, add
`-a codex` or `-a claude-code`. These flags install instructions for the client;
they do not establish a JettWorth connection or guarantee MCP support in that client.
See the [CLI installation reference](https://github.com/vercel-labs/skills/blob/main/docs/commands.md).

For a local checkout, run this from the repository root (also works before these
changes are published):

```bash
npx skills add . --skill jettworth-financial-review
```

You can also ask an agent with installation access:

> Install the JettWorth financial review skill from https://github.com/Durga-Digital/skills/tree/main/skills/jettworth/jettworth-financial-review for this project.

The repository commands and GitHub links require the skills to be published to
the referenced repository. Ordinary browser chat may not support persistent
local skill installation; use your client's supported skill-import mechanism.

## Connect JettWorth separately

1. Sign in to [JettWorth](https://www.jettworth.me) and open **Agent connections**.
   If prompted, enable Pro in Settings, then return to Agent connections.
2. Follow the setup instructions there. Copy the exact MCP server URL and the
   client-specific OAuth details shown for your agent; do not reuse another
   client's registration or substitute a staging endpoint.
3. Complete browser sign-in and review the requested access. Read access is
   sufficient for the first three skills. To save scenarios, explicitly allow
   scenario editing if offered. An existing read-only connection may need to be
   revoked and reconnected to change consent.
4. Enable JettWorth in your agent conversation and ask it to list your saved plans
   or summarise your finances to verify access.

JettWorth's in-app guide currently provides Claude web/desktop setup. Other agents
need their own supported connection setup; contact
[JettWorth support](mailto:jettworth@durgadigital.com?subject=JettWorth%20agent%20connection)
if your client is not listed. Skill installation in Claude Code or Codex is
separate from configuring that client's MCP connection. Do not paste access
tokens, passwords or service-role keys into a skill or chat.

You can revoke access from Agent connections. Pro, active consent and the
connection's enabled tools determine what the agent can do; a skill cannot
expand those permissions.

## Try it

- **Financial review:** “Use JettWorth to review my finances. Explain what changed
  since my previous snapshot and which assumptions matter in my base plan.”
- **Plan comparison:** “Compare my base plan and my saved career-break scenario
  over 30 years. Explain the differences and any projected shortfall.”
- **Stress test:** “For my base plan, compare the current projection with ETF
  growth two percentage points lower, then inflation one point higher. Use the
  same 30-year horizon and don't save anything.”
- **Scenario planning:** “Create a scenario copied from my base plan called
  ‘Six-month break’, with a six-month career break starting 1 July 2027. Resolve
  the household member with me if needed, then compare it with the base plan.”

The skills account for JettWorth's units (AUD cents, gross income and basis
points), inherited events, projection limits, editing conflicts and safe creation
retries. They do not provide bank syncing, snapshot entry, trades, financial
product recommendations or Monte Carlo simulations through MCP. Projections are
educational planning estimates, not guarantees or personal financial advice.

## Updates and maintenance

```bash
npx skills check
npx skills update
```

Tool workflows were checked against JettWorth's MCP implementation on
4 November 2026. Agents should discover the connected schemas because available
tools depend on deployment and consent. For changes, update each affected skill's
`metadata.version`, this catalog and the root catalog. Validate discovery with
`npx skills add . --list`; listing alone does not verify a live account workflow.
