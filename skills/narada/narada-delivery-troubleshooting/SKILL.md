---
name: narada-delivery-troubleshooting
description: Diagnose missing Narada emails, campaign delivery failures, sender DNS problems, suppressions and uncertain send outcomes. Use for evidence-based troubleshooting without resending messages or changing sending controls.
metadata:
  version: "1.0"
---

# Diagnose an email delivery problem

Read [connection, approvals and recovery](references/mcp-workflow.md) before using
Narada. This workflow diagnoses; it does not resend, remove suppressions, resume
sending or alter DNS as a diagnostic shortcut.

## Locate the affected work

Start from the user's message, issue, broadcast, approval or operation ID and
expected time. Resolve ambiguous resources before inspecting recipient data.
Use available tools appropriate to the object:

- `emails_get` with `narada:emails:read` for one message.
- `approvals_get` with `narada:approvals:read` and target access for an owner review.
- `operations_get` / `operations_lookup` with `narada:operations:read` and original
  action access for acceptance or a lost response.
- `issues_get`, `issues_delivery_get`, `issues_recipients_list` with issue read
  and delivery-read scopes for publication email.
- `broadcasts_get` / `broadcasts_recipients_list` with `narada:broadcasts:read`
  for a campaign.

A pending/rejected review has not authorized sending. An accepted request,
completed broadcast, web publication or complete form response does not establish
recipient delivery. Distinguish scheduled, queued, submitted, delivered, bounced,
complained, failed, suppressed and unavailable only as supported by actual data.
Even a provider delivery event does not prove the message was read or reached
the inbox rather than spam. Local capture is not external delivery.

## Narrow the cause

Read `sending_get` (`narada:sending:read`) and `sending_diagnostics`
(`narada:diagnostics:read`) when available for pause/provider readiness.
`domains_get` / `domains_list` use `narada:domains:read` and return saved diagnostics;
state their freshness. If the user requests a fresh check, `domains_refresh`
requires `narada:domains:refresh` and a retry key because it persists diagnostics.
It does not configure DNS. Report exact returned DNS requirements, never guessed
records, and distinguish sender-domain authentication from publication website DNS.

Use `suppressions_get` with `narada:suppressions:read` for a specific authorized
recipient when appropriate. A missing suppression is not positive consent or
proof of eligibility. Workspace sending settings, account diagnostics and
suppressions are unavailable to resource-restricted grants; domain restrictions
can still allow that domain's diagnostics. Do not broaden access silently.

For aggregate context, `metrics_get` needs `narada:metrics:read` and accepts
`query.days` (1–90) and optional `query.domain`. A domain-restricted grant must
specify an allowed domain. Metrics use UTC acceptance dates, and later feedback
can revise earlier counters. Compare matching windows/domains and state the
basis; aggregate rates cannot explain an individual message on their own.

## Report evidence and the next action

Separate observed facts from hypotheses and name missing permissions or records.
Give affected IDs, timestamps/timezone, actual state, strongest supported cause
and the narrow next action. Retain request/operation IDs for support, without
publishing addresses, payloads or secrets.

For uncertain outcomes, follow receipt recovery in the reference. A 404, expired
receipt or `unknown` status never justifies another send. If a repair or deliberate
resend is requested later, treat it as a distinct authorized action with its own
permissions and, for MCP sends, owner review. Suppression removal does not restore
subscription consent, and cancellation cannot recall submitted mail.
