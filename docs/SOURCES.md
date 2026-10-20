# Sources and access notes

Checked during initial scoping on 20 October 2026. These notes are bounded findings, not blanket legal clearance or confirmation of exact deployed source version.

## Access and implementation

- Public API guidance: https://www.whatdotheyknow.com/help/api
- Search syntax: https://www.whatdotheyknow.com/advancedsearch
- House rules: https://www.whatdotheyknow.com/help/house_rules
- Robots policy: https://www.whatdotheyknow.com/robots.txt
- Privacy/research-sharing guidance: https://www.whatdotheyknow.com/help/privacy
- Recent initial-send JSON feed: https://www.whatdotheyknow.com/feed/list/recent.json
- Example request JSON: https://www.whatdotheyknow.com/request/members_of_liverpool_city_counci.json
- Upstream models: https://github.com/mysociety/alaveteli/tree/develop/app/models
- Upstream controllers: https://github.com/mysociety/alaveteli/tree/develop/app/controllers

Ordinary documented public JSON use has not been shown to require an individual prior application. Whole-archive collection/load and HTML collection are separate operational issues. Do not overstate either permission or prohibition. An export can reduce load but is not a prerequisite for V1. Respect explicit blocks and do not bypass controls.

## Observed technical constraints

- Request JSON contains public metadata/event history but not full initial text or word count.
- JSON/Atom feeds contain truncated snippets; they do not replace individual request JSON for complete metadata events.
- Two recent-feed pages each returned 25 distinct initial-send events without overlap.
- Full-year authority/date search retrieved a known 2008 request; narrower date searches missed it. Historical enumeration remains unproven.
- Direct Python requests received HTTP 403 on this machine, whereas ordinary browser/web extraction access worked. A new collector must test actual access and stop on denial rather than evade it.
- `/api/v2/request/:id.json` has full text in upstream implementation but requires an authority-scoped key; it is not the anonymous research read API.

## Related work

- Ben Worthy, June 2026 parliamentary evidence on 2025/2026 FOI officer surveys: https://committees.parliament.uk/writtenevidence/167440/html/
- Maria Zuffova (2025), What the Public Wants to Know, pre-LLM corpus: https://journalqd.org/article/view/8761
- Alex Parsons/mySociety, Public FOI, volume/outcome comparability and classification gaps: https://research.mysociety.org/html/public-foi/
- Shin, Kim and Shin (2025), consumer-complaint analogue: https://insights.som.yale.edu/insights/when-ai-is-the-editor-consumer-complaints-are-more-likely-to-succeed

Do not claim first-ever AI/FOI research or causal AI effects from a date break alone. Contact researchers about unpublished overlap before a strong novelty claim.
