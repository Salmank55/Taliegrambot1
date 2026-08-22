# Example: Telegram daily digest

Use this as a low-cost bot design that separates preview from sending and makes duplicate delivery visible.

## Automation contract

| Field | Decision |
| --- | --- |
| Trigger | Local schedule at a chosen timezone |
| Input | Approved text items from a local queue |
| Action | Build one digest and send it to one authorized chat |
| Output | Message ID plus local delivery record |
| Cost | Local script and one existing bot token; no paid workflow platform |
| Stop control | Disable schedule or set `AUTOMATION_ENABLED=false` |
| Recovery | Checkpoint by date and stable job ID |

## Safe flow

1. Load the queue and validate each item.
2. Compute `job_id = digest:<date>:<chat_id>`.
3. If the job is already marked sent, exit without sending again.
4. Generate a dry-run preview and validate length/format.
5. Send only after the execution mode is enabled.
6. Record a redacted result with status and provider message ID.
7. Move failed items to review instead of deleting them.

## Failure tests

| Test | Expected result |
| --- | --- |
| Same schedule runs twice | One message, one stable job record |
| Token missing | No send; private alert and paused connector |
| Rate limit | Preserve job and retry within a bounded policy |
| Unauthorized chat | Reject before reading or sending private content |
| Malformed queue item | Skip item with review reason |
| Network unavailable | Keep local queue; report a safe temporary failure |
| Stop flag enabled | No new external side effect |

## Operator commands

Document equivalents for `status`, `dry-run`, `run-once`, `retry <job_id>`, `pause`, `resume`, and `cleanup`. Do not require source-code edits to stop the automation.

## Delivery note

Report credentials by name only, never by value. State the schedule timezone, retention period, provider limits observed during testing, and which side effects were tested in dry-run versus live mode.
