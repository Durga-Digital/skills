# Jessse

Skills for working with Jessse Brand context, content and saved research through
an approved MCP connection.

| Skill | Purpose |
| --- | --- |
| [jessse-brand](jessse-brand/SKILL.md) | Use Brand persona, task context, feedback, recurring continuity and supplied performance evidence to manage ideas, drafts, media, campaigns, series, external recurring Brand work, Brand plans, canonical articles/profile copy and approved publishing schedules. |
| [jessse-research](jessse-research/SKILL.md) | Research with your harness's tools using current Brand context, then save findings and sources to Jessse. Includes guidance for external recurring jobs. |

```bash
npx skills add Durga-Digital/skills --skill jessse-brand --skill jessse-research
```

The skills require an existing Jessse MCP connection and explicitly granted
permissions. Brand context and research remain private to that connection; this
repository contains only reusable instructions. Research requires search tools
in your harness. The skill does not start a recurring job by itself.

Brand Operator and series execution belong to your agent. Save preferences in Jessse, then configure and confirm recurrence in the harness. This guidance requires the app release exposing `get_brand_operator`; internal series activation is not a fallback.

Brand plans and long-form tools require the matching app/worker release and fresh Brand-work consent. Skill 1.8 also requires workspace contract 1 for continuation tools and fresh workspace/performance consent. Publish after the matching app migration and release. Existing permitted workflows remain available when the server lacks the new contract.
