---
name: frugal-automation-architect
description: Design reliable, low-cost automations and bots using free, open-source, local, or already-available services. Use for Telegram bots, file workflows, scheduled jobs, webhook integrations, personal productivity automation, API orchestration, and no-paid-tool prototypes.
---

# Frugal Automation Architect

Design small automations that solve a real repeated task without creating a fragile maze of paid services. Prefer local execution and transparent dependencies, but support cloud APIs when the user explicitly provides access and accepts the cost, privacy, and availability trade-offs.

## Convert the idea into an automation contract

Write the contract before choosing a framework:

| Field | Specify |
| --- | --- |
| Trigger | Manual command, schedule, file arrival, webhook, or message |
| Input | Schema, source, size, language, and expected frequency |
| Action | The exact transformation or side effect |
| Output | Destination, format, recipient, and success confirmation |
| Identity | Which account or bot is authorized to act |
| Cost ceiling | Free-only, low monthly limit, or no restriction |
| Privacy | What data may leave the machine |
| Recovery | Retry, queue, manual review, or safe no-op |
| Stop control | How the user pauses or disables the automation |

If the request asks for “automatic” behavior without an explicit stop path, add one before implementation.

## Choose the cheapest dependable topology

Use this decision order:

1. A local script or command is sufficient: choose it.
2. A scheduled local job is sufficient: add a scheduler and persistent logs.
3. A lightweight bot or webhook is needed: use one small service with a durable state store.
4. Multiple external APIs are unavoidable: isolate each connector behind an adapter and document quotas, credentials, and fallback behavior.
5. A hosted persistent service is required: confirm the user’s budget and deployment access before building around it.

Avoid introducing a database, queue, workflow platform, or cloud function unless it solves a demonstrated reliability problem. Do not treat a free tier as a permanent guarantee; record the current assumption and provide a local fallback where feasible.

## Make side effects safe

Design every action to be **idempotent** when possible. Assign a stable event or job ID, record the last successful step, and make retries detect already-completed work. Separate planning from execution so a preview can show what will happen before a message is sent, file is deleted, payment is attempted, or public post is published.

For bots, validate command arguments, authorize the sender, rate-limit expensive actions, escape output for the target platform, and return a clear error without leaking stack traces or secrets. For webhooks, verify authenticity before parsing the payload, reject replays when the provider supports event IDs, and respond quickly before doing slow work.

## Handle failures deliberately

Use bounded retries with backoff only for transient failures. Do not retry invalid credentials, malformed input, permission failures, or destructive actions blindly. Store failed jobs with the reason, attempt count, and next action. A human-review queue is better than an infinite retry loop.

| Failure | Safe default |
| --- | --- |
| Duplicate event | Ignore after confirming the same event ID |
| Timeout | Retry within a cap, then mark for review |
| Rate limit | Honor the provider’s delay and preserve the job |
| Invalid input | Explain the required format and do not mutate data |
| Missing credential | Pause the connector and alert privately |
| Partial completion | Resume from the last checkpoint |
| Unknown exception | Capture a redacted trace and stop the affected job |

Test failure paths before enabling live side effects. Use a dry-run mode that writes intended actions to a local report.

## Protect credentials and personal data

Load secrets from environment variables or a secret store; never place tokens in code, screenshots, logs, commits, or chat messages. Limit each credential to the narrowest scope available and document how to revoke it. Redact authorization headers, cookies, phone numbers, email addresses, and message bodies from logs unless the user explicitly needs them for debugging.

Default to local storage and minimum retention. If an external API receives personal data, name the field, purpose, destination, retention assumption, and fallback. Do not claim encryption, compliance, or provider reliability without verification.

## Keep the automation observable

Write structured logs with timestamp, job ID, connector, action, status, duration, and a redacted error code. Add a health check that distinguishes “process is running” from “automation is successfully processing events.” Record metrics that matter to the user: completed, failed, retried, skipped, and waiting-for-review jobs.

Provide a compact operator guide containing start, stop, status, dry-run, retry, export, and cleanup commands. A user should be able to disable the automation without editing source code.

## Test the complete lifecycle

Use a test matrix that covers normal and adverse conditions:

| Test | Expected result |
| --- | --- |
| First run | Clear setup message and no duplicate side effect |
| Repeated event | Exactly one intended result |
| Restart | State and pending work survive as designed |
| Offline dependency | Local-safe path or honest failure |
| Rate limit | Job is delayed, not lost or duplicated |
| Invalid sender | Request is rejected without data leakage |
| Malformed payload | Safe error and recorded review item |
| Credential rotation | Old credential fails safely; new one works |
| Stop control | New work stops and current work reaches a safe boundary |
| Dry run | No external side effect occurs |

For scheduled jobs, verify timezone, daylight-saving assumptions, lock behavior, and what happens when one run overlaps the next. For bots, test both authorized and unauthorized users.

## Publish a minimal implementation plan

Return a plan that names the trigger, state, connectors, data flow, cost assumptions, secret names, failure policy, test commands, and rollback path. Implement the smallest end-to-end slice first. Do not add a dashboard, AI agent, or multi-service architecture until the basic flow is reliable.

Before enabling production behavior, require explicit confirmation for irreversible actions such as public posting, deleting files, sending bulk messages, changing account settings, or spending money.

## Useful references

- OWASP, “Application Security Verification Standard”: https://owasp.org/www-project-application-security-verification-standard/
- IETF, “HTTP Semantics — Idempotent Methods”: https://www.rfc-editor.org/rfc/rfc9110.html#name-idempotent-methods
- Telegram Bot API documentation: https://core.telegram.org/bots/api
- Twelve-Factor App, “Config”: https://12factor.net/config

## Practical example

Apply this skill to the included **Telegram daily digest** example in `examples/telegram-daily-digest.md`. Start a new task by copying `templates/automation-contract.yaml` and use `assets/demo.gif` as a quick visual summary of the workflow.
