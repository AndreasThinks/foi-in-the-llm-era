# FOI in the LLM era

An independent research project by Andreas Varotsis, affiliated with Newspeak House, examining changes in public freedom-of-information activity during the diffusion of generative AI.

## Research question

How have public FOI participation and request handling changed during the spread of generative AI? Distinguish new requester accounts from changes within existing accounts, and recipient composition from changes within authorities.

V1 uses publicly accessible WhatDoTheyKnow JSON metadata: request dates, requester account keys, receiving authorities, correspondence events, review indicators and recorded outcomes. V2 may add initial-request word counts and language-shift measures if a suitable access route is established.

This is not a classifier of individuals' AI use. A before/after change alone does not establish causation. Keyword frequencies, if later added, measure language shifts rather than prove AI authorship.

## Status

Initial research and implementation plan. A bounded API collector is the first implementation milestone. No whole-archive collection, representative estimates or research findings have been produced by this repository.

See [the project plan](docs/PROJECT_PLAN.md), [data specification](docs/DATA_SPEC.md), and [source/access notes](docs/SOURCES.md).

## Data handling

The public repository contains code and documentation, not raw API responses, requester-level records, cache files, logs or databases. Requester IDs remain potentially identifiable even without names. Store acquired data locally in ignored directories. Retain only necessary fields in analytical tables; exclude names, biographies and text snippets. Public aggregate outputs require disclosure checks.

Use documented public endpoints conservatively. Honour rate limits and Retry-After; stop on access denial, CAPTCHA or explicit blocking. No proxy rotation, token guessing, identity spoofing or anti-bot evasion. Do not send messages or make FOI requests as part of collection.
