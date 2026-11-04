---
name: narada-audience-import
description: Import an explicitly opted-in CSV into a Narada manual segment, or organize existing contacts and memberships while preserving consent. Use for audience onboarding, field mapping and import reconciliation; not campaign sending.
metadata:
  version: "1.0"
---

# Import and organize an audience

Read [connection, approvals and recovery](references/mcp-workflow.md) before using
Narada. Never infer marketing consent from possessing an email list.

## Verify destination and consent

Read access uses `narada:segments:read`, `narada:contacts:read` and, when needed,
`narada:properties:read` / `narada:topics:read`. Imports require all three of
`narada:contacts:import`, `narada:segments:members:write` and
`narada:subscriptions:write`, even without topic IDs. Recovery needs
`narada:operations:read`. These are workspace operations; sender-domain,
form-ID or publication-ID restricted grants cannot use them.

Resolve the destination with `segments_list` / `segments_get`. Import into a
manual segment. Dynamic segments follow rules; newsletter-owned segments/topics
belong to the publication workflow. Use `properties_list` and `topics_list` to
resolve existing definitions; property types and segment kinds are immutable.

Establish the CSV's source, what people opted into and the exact destination/topic
selection. If consent is unknown, inspect and prepare the file locally but stop
before importing or subscribing. Do not invent a consent attestation, treat
membership as consent, or clear opt-outs to increase reach.

## Prepare the import

Inspect the connected import schema before converting columns. Use a lowercase
`email` header in the first column; other built-ins are `name`, `first_name` and
`last_name`. Custom columns must match existing property keys. Headerless input
is interpreted as email/name. Numbers must parse as numbers and booleans use
`true` / `false`. Duplicate email rows are normalized to lowercase and the last
row wins, so resolve conflicting duplicates before import.

Check headers, email validity, duplicate rows, incompatible property values and
row count; show a compact mapping/error summary without dumping addresses.
Narada currently bounds CSV input to 180,000 characters and 1,000 contacts per
import; respect destination capacity too. Do not silently truncate. A larger
list requires an explicit feasible plan, not parallel imports to bypass capacity.

Call `segments_import` with `params.id` and
`body: {csv, consentSource, consentConfirmed: true, topicIds?}` only when the
consent assertion is supported. Persist the key and exact payload privately
before calling. Report the returned outcome, including `added`, `duplicates` and
`total` when returned. Invalid rows can reject the import rather than being
skipped. Do not equate submitted rows with newly imported or eligible contacts.

## Existing contacts and recovery

`segments_members_add` / `segments_members_remove` change manual membership,
not subscription consent. Addition requires an existing contact and accepts no
body or `{}`; do not attach a consent assertion. `contacts_subscribe` is a
separate preference action requiring source and confirmation, used only when
requested and supported by the person's consent. Never reset other preferences.

When editing a profile with `contacts_update` (`narada:contacts:update`), first
read it: that operation replaces the complete profile and requires name fields,
properties and revision. Preserve unrelated values. Do not delete contacts,
properties or topics as an incidental cleanup step.

CSV import is multi-step. A lost response can leave some profiles, memberships
or preferences committed. An `unknown` operation is fenced against re-execution:
inspect `operations_lookup`, `segments_members_list` and relevant contacts, then
report what is known and the reconciliation needed. Do not submit the same CSV
with a new key. A completed import can replay its saved result using its original
key/input. Membership verification still does not prove send eligibility.
