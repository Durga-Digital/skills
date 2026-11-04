---
name: narada-campaign
description: Prepare standalone Narada broadcast campaigns from drafts or published templates, preview personalization, and request owner approval for tests, sending or scheduling. Use for a campaign to an existing audience rather than a newsletter issue.
metadata:
  version: "1.0"
---

# Prepare a broadcast campaign

Read [connection, approvals and recovery](references/mcp-workflow.md) before using
Narada. Drafting a campaign does not authorize launching it or subscribing people.

## Access and audience

Use `narada:broadcasts:read` and `narada:broadcasts:write` for drafts;
`narada:segments:read` and `narada:topics:read` help resolve the audience.
Templates require `narada:templates:read`. Tests and launch have separate
`narada:broadcasts:test` / `narada:broadcasts:send` scopes plus approval scopes.
Use `narada:operations:read` for recovery. Generic campaign tools require
workspace access without sender-domain, form-ID or publication-ID restrictions.
If excluded by restrictions, explain the limitation; do not repurpose a linked
newsletter broadcast to escape it.

Resolve existing segments/topics and inspect the target broadcast using
`segments_list`, `segments_get`, `topics_list`, `broadcasts_list` and
`broadcasts_get` as needed. Membership, a contact profile or a CSV address is not
consent. Preserve suppressions, global opt-outs and topic preferences. Report
candidate audience size separately from actual send eligibility.

## Prepare an independent draft

Use `broadcasts_create` / `broadcasts_update` with the current schema and revision.
If reusing a template, use `templates_list` / `templates_get` and
`templates_broadcasts_create`; this requires a published broadcast template and
creates an independent draft. Later template edits do not update that draft.
Template publication creates an immutable version and does not launch a campaign.

Check sender, subject, content, links, audience selection, footer and subscription
settings. Use `broadcasts_preview`, optionally with its `query.contact` field for
an authorized personalization example. Check missing variable behavior and avoid
inventing recipient properties. Source HTML/text are public DTO fields; do not
rely on private dashboard editor documents. Return the saved draft for a
prepare-only request.

## Test, schedule or send

A requested test uses owner approval for `broadcasts.test` with the exact test
recipient and any preview contact allowed by the schema. For launch, prepare
`broadcasts.send` with `{revision, confirmed: true, scheduledAt?}` in `body`.
Scheduling uses Unix milliseconds within the server's supported window (currently
90 days). Resolve timezone ambiguity and show the intended local time.

Use `approvals_request`, present the review URL, and then inspect `approvals_get`.
Approval executes the send: never invoke `broadcasts_send` afterward. If the
preview or audience changes, reconcile the existing review before requesting a
new one. A passed test does not approve a production launch.

Read `broadcasts_get`, `broadcasts_recipients_list` and linked operation status
as permitted. Report draft/review/accepted/scheduled/delivery states separately,
with broadcast ID and unresolved failures. `broadcasts_cancel` requires
`narada:broadcasts:cancel` and a user request to stop remaining work; it cannot
recall submitted messages.
