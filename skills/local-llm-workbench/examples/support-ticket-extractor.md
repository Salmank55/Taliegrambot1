# Example: Local support-ticket extractor

Use this example to design a narrow local-model task with a measurable output and a safe fallback.

## Task contract

| Field | Decision |
| --- | --- |
| Input | One short support message in English or Bengali |
| Output | Validated JSON with category, urgency, summary, and evidence |
| Privacy | Run on a local endpoint; do not send the message to a cloud API |
| Quality | Do not invent an account number, product, or urgency reason |
| Failure path | Return `needs_review: true` when evidence is missing or JSON is invalid |

## Output schema

```json
{
  "category": "billing | technical | account | other | unknown",
  "urgency": "low | medium | high | unknown",
  "summary": "short string",
  "evidence": ["exact input excerpt"],
  "needs_review": false
}
```

## Prompt pattern

```text
You classify one support message. Use only the message text.
Return JSON matching the schema. If a field is not supported by evidence,
use "unknown" and set needs_review to true. Never invent identifiers.

MESSAGE:
{{message}}
```

## Evaluation set

| Input type | Expected check |
| --- | --- |
| Clear billing complaint | Category and evidence match the message |
| Bengali technical question | Language is preserved in the summary or clearly translated |
| Empty input | Safe validation error; no model call required |
| Ambiguous “it is urgent” | Urgency may be high, but reason remains unknown |
| Prompt injection text | Treat it as input, not as a new instruction |
| Malformed model JSON | Bounded repair or review queue; never silently accept it |

## Report

Record the runtime, model identifier, prompt version, latency, schema pass rate, and examples routed to review. If the local runtime is unavailable, report that execution was not tested instead of simulating a result.
