# V1 data specification

## Request records

Stable request ID; first sent-event time and request creation time separately; requester account ID or null; receiving authority ID/name; `law_used`; `described_state`; `display_status`; `awaiting_description`; distinct incoming-message count; distinct outgoing follow-up count; fetch/snapshot time and source URL. Authority tags may be current rather than historical.

## Event records

Request ID; event ID; event type; creation time; described/calculated state; `last_described_at`; incoming/outgoing message IDs; optional display status. Store event IDs for chronological tie-breaking. Exclude snippets, names, biographies, email addresses and raw administrative params from analytical exports.

## Derivations

Count distinct incoming message IDs on response events. Count distinct outgoing follow-up IDs from followup send/resend events, excluding initial sends. Administrative edits, classifications, comments and deadline events are not correspondence. Resending one outgoing message must not count as a new authored message.

Stored waiting-response state can coexist with displayed Long overdue or Awaiting classification. Retain all fields. Classification timestamps can be later than the underlying response; never relabel classification time as actual disclosure time. Older records can carry classification on correspondence events without standalone status-update rows.

## Excluded from V1

Full message text, attachments, initial-request word counts, AI-authorship labels and protected/inferred demographics. Snippets are truncated and omit portions of some requests; never use snippet length as initial-request length. Requester IDs are account identifiers, not unique people or anonymous data.

## Local reference expectations

Previously observed purposive examples, to validate against fresh live data rather than hard-code as collection output:

- 253: successful; 4 incoming messages, 2 follow-ups.
- 1511379: successful; 4 incoming, 1 follow-up; two status updates including not-held then successful.
- 1503019: partially_successful; 2 incoming, no follow-ups.

These are snapshots and can change. A changed live record should trigger inspection, not fabricated matching output. No sampling representativeness is implied.
