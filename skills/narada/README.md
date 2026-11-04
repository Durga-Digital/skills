# Narada skills

Give your agent repeatable workflows for Narada's newsletters, campaigns, forms
and email infrastructure. Start with the skill matching your task; each is
independently installable and includes its own approval/recovery guidance.

| Skill | Use it for | Access |
| --- | --- | --- |
| [narada-newsletter](narada-newsletter/SKILL.md) | Draft, preview, test and publish a newsletter issue to web, email or both. | Publication/issue read and editing; channel and approval permissions for release |
| [narada-campaign](narada-campaign/SKILL.md) | Prepare a standalone broadcast, reuse a published template, and schedule or launch with owner review. | Workspace audience/campaign access; separate test/send and approval permissions |
| [narada-audience-import](narada-audience-import/SKILL.md) | Import opted-in contacts into a manual segment and reconcile partial imports without losing consent history. | Workspace reads; import, membership and subscription permissions |
| [narada-forms](narada-forms/SKILL.md) | Build intake/feedback/signup forms, publish, analyze responses and export answers. | Form editing/publishing; separate response/export and integration permissions |
| [narada-delivery-troubleshooting](narada-delivery-troubleshooting/SKILL.md) | Explain missing emails, delivery failures, DNS readiness and uncertain send outcomes. | Relevant message/delivery/diagnostic reads; optional diagnostic refresh |
| [narada-app-integration](narada-app-integration/SKILL.md) | Add transactional email to an app or migrate from Resend with sender setup and recovery-aware code. | Integration guide, optional domain setup, and access to the consuming repository |

These skills add workflow judgment around the MCP tools: audience membership is
not consent, a saved draft is not a release, and accepted mail is not delivered
mail. Newsletter publishing and campaign launch are separate workflows so an
agent does not reach for generic broadcasts to bypass publication restrictions.

## Install

Use Node.js 22.20 or later and the
[Skills CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md).
From a local checkout, the new skills can be listed and installed immediately:

```bash
# Run from the skills repository root; list without installing
npx skills add . --list

# Install into your current project, choosing the agent interactively
npx skills add . --skill narada-newsletter
```

Install from the shared repository:

```bash
# Browse the public catalog
npx skills add Durga-Digital/skills --list

# Install one workflow
npx skills add Durga-Digital/skills --skill narada-newsletter

# Install all six Narada workflows
npx skills add Durga-Digital/skills \
  --skill narada-newsletter \
  --skill narada-campaign \
  --skill narada-audience-import \
  --skill narada-forms \
  --skill narada-delivery-troubleshooting \
  --skill narada-app-integration

# Make one workflow available across projects in Codex
npx skills add Durga-Digital/skills --skill narada-newsletter -a codex -g

# Or Claude Code
npx skills add Durga-Digital/skills --skill narada-newsletter -a claude-code -g
```

Choose only the workflows you need. Installing all six does not grant account
access or require granting all permissions. You can also ask an agent with
installation access:

> Install the Narada newsletter skill from https://github.com/Durga-Digital/skills/tree/main/skills/narada/narada-newsletter for this project.

Persistent local installation depends on the client. Browser chat may require
its own supported skill import mechanism. Skill installation is separate from
connecting your Narada account.

## Connect Narada separately

1. Sign in to [Narada Cloud](https://app.sendnarada.com) and open
   **Profile → Security → Connected apps**.
2. Add `https://app.sendnarada.com/api/mcp` as a remote HTTP MCP server in your
   agent's MCP settings. Self-hosted installations use their own endpoint.
   Your client must support remote HTTP MCP and browser OAuth; installation of a
   skill alone does not establish this connection.
3. Complete Narada's browser sign-in and consent. Choose the workflow's required
   scopes and, where supported, restrict access to the intended resources.
   Read/write/test/send/export scopes are independent. For MCP tests and sends,
   include `narada:approvals:request`, `narada:approvals:read` and the target
   action's permissions. `narada:operations:read` supports recovery.
4. Ask the agent to discover its tools and run an available read-only check such
   as `whoami`, `capabilities` or listing your permitted publications. Those
   identity/capability checks require their respective read permissions.

Existing grants do not automatically gain new permissions. Reconnect through
consent when required, and manage or revoke access in Connected apps. Workspace
campaign/audience tools cannot be used with sender-domain, form-ID or
publication-ID restrictions; use scoped issue/form tools for those resources.
An absent tool may reflect permissions or the deployed server version.

MCP uses OAuth, not your application's API key. Credentials and account security
remain owner-managed. Never paste keys or tokens into a skill or chat.

For a send/test/issue publication, the agent prepares the exact work and returns
Narada's owner review URL. **Approving in Narada executes the operation.** The
agent then checks status; it must not send a second time. The same rule applies
to web-only issue publication. Application backend sends using API keys retain
their separate unattended behavior within the key's permissions.

## Try it

- **Newsletter:** “Draft this week's issue for Studio Notes from these release
  notes. Save it and show me a preview; don't publish yet.”
- **Campaign:** “Prepare a broadcast for our existing beta-testers segment using
  the published launch template. Preview the personalization and get it ready
  for my review.”
- **Audience import:** “Check this opted-in CSV for my manual Customers segment.
  Show the field mapping and invalid rows before importing. Preserve opt-outs.”
- **Forms:** “Draft a client feedback form without contact sync or acknowledgment
  emails. Show me the preview before publishing.”
- **Delivery troubleshooting:** “This message ID was accepted but the customer
  hasn't received it. Inspect its delivery state and sender diagnostics.”
- **App integration:** “Migrate this app's Resend transport to Narada. Preserve
  templates and retry behavior, run local tests, and leave deployment for later.”

## Availability and maintenance

Workflows were checked against Narada's local canonical contracts and MCP
implementation on **4 November 2026**. The source includes changes beyond earlier
deployments. Skills must discover the connected server's schemas and permissions;
this catalog does not certify a live deployment, every hosted workflow, or an npm
SDK release. The app integration skill checks package availability before using
SDK features. These files are instructions, not new dashboard/API/SDK/MCP features.

The initial set deliberately omits dedicated skills for newer publication-site,
branding and audience-deletion operations. It also excludes credential/account
administration, automatic owner approval and unrequested production cutovers.
DNS-provider writes need separate authorization and tooling. There is no need to
install a separate recovery skill: each workflow bundles that guidance.

Each `references/mcp-workflow.md` copy is intentional so a single skill works
alone. Keep these copies aligned when shared behavior changes, and increment each
affected skill's `metadata.version`. Update this catalog and the root catalog.
Validate with the [Agent Skills format](https://agentskills.io/specification),
frontmatter/link checks and `npx skills add . --list`. Listing is discovery
validation, not a live send or import test.

```bash
npx skills check
npx skills update
```
