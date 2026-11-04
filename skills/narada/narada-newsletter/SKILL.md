---
name: narada-newsletter
description: Draft, preview, test and publish Narada newsletter issues, including scheduled web and email releases with owner approval. Use for a publication issue rather than a standalone broadcast campaign.
metadata:
  version: "1.0"
---

# Prepare and publish a newsletter issue

Read [connection, approvals and recovery](references/mcp-workflow.md) before using
Narada. A draft-only request ends with a saved draft and preview.

## Access and target

For an existing newsletter, use `narada:publications:read`, `narada:issues:read`
and, when editing, `narada:issues:write`. Add `narada:issues:test` for a requested
test, `narada:issues:publish` for web, `narada:issues:send` for email, and both
for both channels. Approval scopes are required for tests and publication.
`narada:issues:delivery:read` allows delivery inspection;
`narada:subscribers:read` allows subscriber inspection. Prefer a grant restricted
to the intended publication. Operations recovery uses `narada:operations:read`.

Resolve the publication with `publications_list` / `publications_get`, then the
issue with `issues_list` / `issues_get`. Verify sender, current revision and
requested channel. Use `publications_subscribers_list` for newsletter subscription
history when needed; generic segment membership is only a candidate pool and
cannot establish newsletter consent. Do not create a second publication to work
around a restricted grant or unavailable target.

## Draft and preview

Use `issues_create` or `issues_update`; updates take `{draft, revision}` under
`body` and `{id, issueId}` under `params`. Here `id` is the publication ID.
Use the schema's structured document format; do not assume an arbitrary HTML
string is an issue document. Preserve content outside the requested edit.

Call `issues_preview`. Check links, subject/title, sender, requested content and
web/email presentation. Surface missing facts or assets rather than inventing
claims. A preview does not send mail. For a requested test, request owner approval
for `issues.test` with the exact test recipient and current revision.

## Release and report

For a requested release, prepare `issues.publish` with current `revision`,
`channels: "web" | "email" | "both"`, `confirmed: true`, and optional
`scheduledAt` in Unix milliseconds. Resolve ambiguous timezone/date before
scheduling; state the local time and timezone in the handoff. Use the approval
workflow in the reference, including for web-only releases.

After owner approval, inspect `issues_get` and, for email when permitted,
`issues_delivery_get` / `issues_recipients_list`. Separate acceptance, web
visibility and recipient delivery. Report the issue ID, saved revision, preview
or public URL when returned, chosen channels, schedule and remaining review.

An issue already distributed by email cannot be resent. `issues_duplicate`
creates a new draft for an intentionally new distribution, never for recovering
an uncertain send. A requested schedule cancellation uses `issues_cancel` with
`narada:issues:cancel`; web withdrawal uses `issues_withdraw` with
`narada:issues:withdraw`. Neither recalls email already submitted.
