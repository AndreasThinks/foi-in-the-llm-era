# FOI in the LLM era: project and implementation plan

**Goal:** Produce a reproducible V1 descriptive study of public FOI activity and handling using WhatDoTheyKnow's public JSON metadata.

**Architecture:** A single-worker, rate-limited Python collector discovers request URLs using documented JSON feeds/search, then retrieves individual request JSON for event histories. A local SQLite store supports caching, resumability, deduplication and bounded export. Keep all acquired data outside Git.

**Tech stack:** Python 3.11+, uv, HTTP client, SQLite, pytest; choose minimal dependencies. Codex implements; Avery independently verifies.

## Publication scope

Publish this plan and later reviewed code. No raw public API payloads, requester-level outputs or operational logs in Git. No open licence has been selected; do not add one without the owner's decision. No affiliation endorsement, funding, ethics approval or causal finding is implied.

## Research scope

V1 outcomes: request counts over time; new/returning requester accounts; activity within requester; receiving authority mix; incoming correspondence and outgoing follow-ups; review transitions; recorded outcome trajectories and time-to-event descriptions. Distinguish accounts from people and acknowledgements from substantive disclosure.

Initial specification uses all history in principle, but a bounded smoke test is not an archive census. Validate discovery before a larger run. V1 omits length and text analysis. V2 can add initial-request lengths and possibly fixed-dictionary language-shift measures, with explicit limits on AI attribution.

## Task 1: package and offline parser

Create `pyproject.toml`, `src/foi_llm/__init__.py`, `src/foi_llm/normalize.py`, `tests/test_normalize.py`. Establish a checkout-local real `.venv` using uv. Write failing tests before implementation, then implement request/event field extraction.

Canonical check: `uv run pytest -q`.

Tests must cover initial sends versus follow-ups; duplicate/re-sent message IDs; missing requester; unclassified response; administrative/comment/deadline events; distinct stored and display statuses; and retroactive classification timestamps. Small explicitly synthetic fixtures are allowed only for offline tests and must not be represented as real collection results.

## Task 2: local persistence and provenance

Create `src/foi_llm/store.py` and `tests/test_store.py`. Persist requests, events, acquisition status and discovery checkpoints in local SQLite. Use transactional upserts keyed by stable IDs and cache by URL with fetch timestamps/status. Keep raw responses only under ignored local data/cache if needed. Avoid names and text in normal logs. Repeating a completed bounded collection should not duplicate records or silently refetch everything.

## Task 3: conservative JSON HTTP client

Create `src/foi_llm/client.py` and `tests/test_client.py`. Default one worker and at least two seconds between live requests. Configurable descriptive User-Agent, timeouts, finite retries on transient 5xx/network failures, Retry-After-aware 429 handling and a bounded maximum wait/attempt budget. Stop and record a blocker on 401/403/CAPTCHA/non-JSON challenge pages. No retry storm or block evasion. Record failures as failures; never substitute example records.

## Task 4: discovery and CLI

Create `src/foi_llm/cli.py`, `src/foi_llm/discovery.py`, `tests/test_discovery.py`, and `tests/test_cli.py`. Suggested interface: `uv run foi-llm collect --request-url URL ... --db data/smoke.sqlite`; `uv run foi-llm discover --query QUERY --max-pages N --max-requests N --db data/smoke.sqlite`; `uv run foi-llm export --db ... --output-dir data/exports`; `uv run foi-llm coverage --db ...`.

Explicit URL mode is the required first vertical slice. Discovery uses `/feed/list/recent.json?page=N` or `/feed/search/<query>.json`; deduplicate requests and fetch `/request/<slug>.json` for full metadata histories. Do not treat feed events as complete status history. Preserve a finite request budget including retries. Support resume and an explicit refresh switch. Document exact actual CLI names if different.

## Task 5: historical discovery diagnostics

Write tests for URL encoding preserving date slashes, duplicate/moving pagination, truncated feeds and absent totals. Avoid silently assuming a date window is complete. A known 2008 request appeared in broad-year queries but not two narrow date queries. Explain what was tested and whether date semantics can be resolved; do not launch a broad investigation instead of shipping the explicit-URL collector.

Add a coverage report: query/URL, period, pages, unique IDs, failures, duplicates and whether completeness is unverified. List/sample counts are not population denominators. If requested bounds hit a cap, report truncation rather than complete.

## Task 6: bounded real smoke test

After offline tests pass, attempt a live run of at most 12 HTTP requests total, including retries, to fetch these known public JSON records:

- `/request/car_parking_charges_policy_and_a.json` (ID 253)
- `/request/members_of_liverpool_city_counci.json` (ID 1511379)
- `/request/school_admission_cut_off_distanc_23.json` (ID 1503019)

Use default conservative rate. If permitted ordinary HTTP access is blocked, stop with the exact HTTP/error category and do not work around it. Offline parser/store tests can still pass, but a fixture run is not live success. Parent may independently exercise the client later.

Check real IDs, directional counts, status histories and cached second run. Run `uv run pytest -q` independently, and produce a local report in `artifacts/implementation-report.md` with commands, exit codes, what was actually retrieved, Git diff summary and blockers. Do not publish acquired data. Do not claim successful collection unless it happened.

## Analysis milestones after the collector

1. Prove and document discovery coverage for a predeclared historical scope.
2. Freeze a dated local dataset and data dictionary.
3. Plot descriptive time series and requester/authority decompositions.
4. Use seasonality/trend adjustments, alternative breakpoints, placebo dates and equal follow-up horizons; handle unresolved/unclassified cases explicitly.
5. Check platform policy, embargo/publication and authority-composition changes. Recorded success is not independently verified information yield.
6. Publish reviewed aggregates and uncertainty, then approach mySociety for V2 fields/access and ask about overlapping unpublished work.

## Acceptance gates

- Executable documented CLI, not a stub.
- Offline test suite passes in a local uv environment.
- Explicit bounded HTTP budget, rate limiting, restart safety and idempotent persistence are tested.
- Live smoke test is either independently reproduced or explicitly blocked, with no invented data.
- No raw/requester-level data or secrets in the public Git tree.
- No unbounded archive crawl starts without a separate operational decision.
