---
name: narada-app-integration
description: Integrate Narada transactional email into an application or migrate an existing email provider such as Resend. Use for repository changes, sender setup and end-to-end verification through Narada MCP guidance; requires access to the consuming app code.
metadata:
  version: "1.0"
---

# Integrate an application with Narada

Read [connection, approvals and recovery](references/mcp-workflow.md) before using
Narada. This skill needs the consuming repository and coding tools in addition
to MCP. Narada supplies setup operations and guidance; it cannot edit or deploy
the application itself.

## Inspect before changing transport

Read `integration_guide` and `capabilities` with `narada:capabilities:read` for the
connected installation's current integration contract. The guide may also be
exposed as `narada://guides/project-integration`. Use its actual SDK/HTTP examples;
verify a compatible SDK version is published before adding a package dependency.
Local source support is not evidence of npm availability.

Inspect current send calls, templates, sender addresses, environment variable
names, auth hooks, webhook signature verification, retries and receipt storage.
Map the used provider features; identify unsupported behavior instead of silently
dropping it. Preserve the user's framework and template renderer. A migration
must account for provider-specific webhook payloads and signatures.

## Sender and credentials

Read existing domains with `domains_list` / `domains_get`
(`narada:domains:read`). Use `resources_search` if available to resolve names,
with the selected resource kind's read scope. Reuse valid setup.

If sender registration is requested and exposed, `domains_create` requires
`narada:domains:create`. It registers the hostname with the sending provider;
it does not purchase a domain or edit DNS. `domains_mailFrom_update` and
`domains_dkim_enable` need `narada:domains:configure` and current revision.
New domain registration normally requires unrestricted sender-domain access;
form/publication-only grants cannot administer domains.

Give the exact returned DKIM and MAIL FROM records, including type/name/value.
Preserve inbound MX routing, existing provider records and DMARC policy. DNS
changes need a separately authorized DNS tool or owner action. After propagation,
use `domains_refresh` with its scope/key and inspect sending readiness when allowed.
Local simulated diagnostics cannot verify public DNS.

The owner provisions a sending key restricted to the exact From domain and
places it in the application's server-side secret store. Preserve a valid existing
key. MCP cannot mint/retrieve credentials or manage account security. Never ask
for secrets in chat, commit them, place them in client code, or use an application
key to bypass MCP's approval flow.

## Implement and verify

Adapt the app using the connected guide. Preserve template variables, auth-hook
verification, token lifetime and the application's durable mapping from business
event to retry key. Retries of the same logical send must reuse its key and exact
payload; do not generate a fresh key inside each retry attempt. Handle acceptance
separately from delivery and process duplicate webhook events idempotently.

API-key application sends execute within that key's existing permissions; they
are not MCP sends. A test initiated through MCP uses owner approval for
`emails.send` (or the matching test operation) as described in the reference.
Do not introduce a duplicate application send after an approval has executed.

Run appropriate isolated tests for rendering, retries, provider errors, webhook
verification and auth callbacks. For OTP/magic-link/password-reset flows, a send
acceptance is not a verified login: test the callback/token validation through to
the expected completed session in the approved test environment. Report checks
not run. Deploy or cut over only when the user has authorized that step; keep the
existing provider path recoverable until the intended application outcome is
verified. Report code changes, setup still needed, test evidence and release state.
