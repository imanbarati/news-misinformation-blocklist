# Handoff: Multilingual News Misinformation & Impersonation Blocklist

**Target repository:**
https://github.com/imanbarati/news-misinformation-blocklist\
**Primary goal:** Build and maintain a broad, evidence-based,
multilingual domain blocklist for ad-blocking and DNS filtering tools.\
**Languages in scope:** English, Chinese, and Persian (Farsi).\
**Geographic scope:** United States, China, Iran, Persian-language
diaspora outlets, and international publishers.

> **Handoff rule:** Do not claim that work has been pushed unless a
> commit is visible in the target GitHub repository. The previous GitHub
> connector attempts failed with internal errors; no successful commit
> was confirmed. Start by inspecting the repository and its default
> branch.

------------------------------------------------------------------------

## 1. Mission and scope

Create a useful, maintainable blocklist of domains that have credible
evidence of repeatedly publishing fabricated news, materially deceptive
content, conspiracy claims presented as fact, or operating as
impersonation domains.

The user wants broad coverage: major, medium-sized, and small
English-language/American, Chinese, and Persian-language news-related
sites. Broad coverage does **not** mean indiscriminately blocking every
publisher in those countries or languages.

The list must be suitable for integration with: - uBlock Origin -
AdGuard - Pi-hole - Hosts-based DNS blocking - Other domain-list
consumers

Provide separate, clearly named feeds so users can choose conservative
or more aggressive filtering.

## 2. Important editorial correction: Factnameh

The user explicitly states that **Factnameh is fake news** and does not
want it treated as a trusted fact-checking authority.

Handle this carefully: - Do **not** list Factnameh as an authoritative
or trusted verification source. - Do **not** use Factnameh as the sole
basis for adding or removing any domain. - Do **not** automatically add
Factnameh's domain to a blocklist merely because the user made this
assertion. - Record the user's assessment in project notes as an
**unverified user-provided allegation**, pending independent evidence
and review. - If Factnameh itself is ever evaluated as a candidate
domain, assess it under the exact same published criteria used for every
other publisher. Preserve evidence, dates, and reviewer rationale. - Do
not repeat earlier project notes that describe Factnameh as nonpartisan
or authoritative. That characterization is disputed by the user and has
not been independently established in this project.

A domain's political position, ownership, nationality, or disagreement
with a fact-checker is not by itself proof of fabricated reporting.

## 3. Classification policy

Never use one vague label such as `fake_news` for every type of concern.
Maintain explicit categories.

### Eligible for block feeds when evidence is sufficient

-   `fabricated`: repeated publication of demonstrably invented stories
    or fabricated events.
-   `unreliable`: repeated material factual deception, with documented
    examples and a clear pattern.
-   `conspiracy`: persistent promotion of demonstrably false conspiracy
    narratives as factual reporting.
-   `propaganda`: coordinated deceptive influence activity or systematic
    disinformation, supported by documented evidence. State ownership
    alone is insufficient.
-   `impersonation`: domains impersonating legitimate publishers, public
    institutions, or news brands.

### Metadata / research labels; not default hard-block categories

-   `satire`: clearly identified satire or parody.
-   `bias_left`, `bias_right`, `bias_mixed`: editorial orientation; not
    proof of falsehood.
-   `state_owned`: ownership/funding metadata only.
-   `partisan`: political alignment metadata only.
-   `unknown`: insufficient evidence or unresolved status.

Do not place satire, ordinary political bias, state ownership, or merely
controversial publishers in the default hard-block feed.

## 4. Evidence standard

Every listed domain must have a traceable reason for inclusion.

For each record, capture as many of these fields as possible:

  -----------------------------------------------------------------------
  Field                               Meaning
  ----------------------------------- -----------------------------------
  `domain`                            Normalized registrable domain;
                                      lowercase, no scheme or path

  `category`                          One primary classification from the
                                      policy above

  `language`                          `en`, `zh`, `fa`, or other
                                      applicable language code

  `country`                           Country or `international`; keep
                                      separate from credibility

  `evidence_urls`                     Direct links to evidence supporting
                                      the classification

  `evidence_summary`                  Short, factual explanation; avoid
                                      political insults

  `first_seen`                        Date first added, ISO 8601

  `last_reviewed`                     Date last checked, ISO 8601

  `confidence`                        `high`, `medium`, or `low`

  `source_names`                      Research sources used, with
                                      provenance

  `review_status`                     `verified`, `needs_review`, or
                                      `rejected`

  `notes`                             Additional context, including
                                      ownership or impersonation target
  -----------------------------------------------------------------------

Minimum rule for a normal publisher domain: 1. Prefer multiple
independent, credible evidence items. 2. Include direct examples and
dates, not only a list entry from another blocklist. 3. Check whether
the cited article was corrected, retracted, satirical, mistranslated, or
taken out of context. 4. Distinguish a fabricated story from a
legitimate report that was later found inaccurate. 5. Record uncertainty
and allow appeal/removal. 6. Do not add domains solely to make the list
look larger.

For impersonation domains, document the legitimate entity being
impersonated and the evidence that the domain is deceptive.

## 5. Required repository structure

Create or adapt this structure after inspecting the existing repository.
Preserve useful existing work; do not overwrite files blindly.

``` text
.
├── README.md
├── HANDOFF.md
├── LICENSE
├── CONTRIBUTING.md
├── CHANGELOG.md
├── data/
│   ├── domains.csv
│   ├── domains.json
│   └── schema.json
├── lists/
│   ├── conservative.txt
│   ├── aggressive.txt
│   ├── ublock-origin.txt
│   ├── adguard.txt
│   ├── pihole.txt
│   └── hosts.txt
├── languages/
│   ├── en.txt
│   ├── zh.txt
│   └── fa.txt
├── countries/
│   ├── us.txt
│   ├── cn.txt
│   ├── ir.txt
│   └── international.txt
├── categories/
│   ├── fabricated.txt
│   ├── unreliable.txt
│   ├── conspiracy.txt
│   ├── propaganda.txt
│   └── impersonation.txt
├── sources/
│   ├── SOURCES.md
│   ├── ATTRIBUTION.md
│   └── source-review.csv
├── scripts/
│   ├── normalize_domains.py
│   ├── validate_data.py
│   ├── deduplicate.py
│   └── build_lists.py
└── .github/
    └── workflows/
        └── validate.yml
```

Use the repository's existing conventions if they are better. The file
name may be `handoff.md` in lowercase if that matches the repository
style; this document is intended to be committed as `handoff.md`.

## 6. Output formats

Generate feeds from the canonical dataset; do not manually maintain
multiple divergent copies.

### Plain domain list

One domain per line. No comments in feeds intended for strict consumers
unless the target format supports comments.

### uBlock Origin

Use valid network-filter syntax, for example:

``` text
||example.invalid^
```

### AdGuard

Use compatible domain-blocking syntax and validate against current
AdGuard list conventions.

### Pi-hole

Provide a plain domain list suitable for gravity ingestion. Include a
short installation/update section in the README.

### Hosts

Use:

``` text
0.0.0.0 example.invalid
```

Do not add wildcard or subdomain rules unless evidence and format
semantics justify them. Document whether a parent-domain entry is
intended to cover subdomains.

## 7. Language and regional coverage

Maintain independent feeds for English, Chinese, and Persian. Keep
language separate from country: a Persian-language publisher may be
based outside Iran, and an English-language publisher may be based in
China or elsewhere.

### English / United States / international

Research major, medium, and small publishers and deceptive domains. Do
not equate conservative/liberal editorial positions with misinformation.
Include impersonation domains separately.

### Chinese

Research Simplified and Traditional Chinese-language sources, domains
targeting audiences in mainland China, Taiwan, Hong Kong, and
Chinese-speaking communities abroad. Do not classify a site as
misinformation merely because it is state-owned, dissident,
foreign-funded, or politically aligned.

### Persian (Farsi)

Research: - Iran-based Persian-language publishers - Persian-language
diaspora publishers - Pro-government and anti-government sources using
identical standards - Fake domains impersonating established
Persian-language broadcasters and newspapers - Fabricated-news websites
and recurring deceptive networks - Persian-language social/news
aggregators where domain-level evidence exists

Do not assume that a fake domain impersonating BBC Persian, VOA Persian,
or another outlet makes the genuine outlet fake. Put the deceptive
lookalike domain in `impersonation` and record the impersonated brand.

## 8. Research sources and licensing

Use source datasets as discovery leads, not unquestionable truth.
Independently verify candidates before inclusion.

Potential research inputs already identified:

-   **CRED-1**: an open credibility dataset containing domain-level
    categories such as fake, unreliable, mixed, conspiracy, satire, and
    rumor. Its documentation describes a CC BY 4.0 dataset and
    MIT-licensed code. Verify the current repository, exact license,
    version, and attribution requirements before importing.
-   **Jana Lasser's misinformation_domains**: a collection aggregating
    domains from multiple sources. Its documentation warns that upstream
    sources can have different or missing licenses. Do not assume the
    compilation grants redistribution rights for every upstream list.
-   **Access Now, "Fake Domain Attacks"**: research useful for
    identifying impersonation patterns and examples targeting
    Iranian/Persian media. Treat it as evidence about impersonation, not
    proof against the authentic publisher being impersonated.
-   Independent academic research, transparent media-reliability
    projects, documented corrections/retractions, court or regulator
    findings where relevant, and direct primary evidence.

Do not scrape or redistribute a source dataset if its terms do not
permit it. Prefer linking to sources, retaining only independently
verified records, and recording attribution.

### Factnameh handling

Factnameh is **not an approved trusted-source input for this project**
unless and until it passes independent review. The user's claim that it
is fake news is an allegation to investigate, not a pre-verified
conclusion. Never rely on it as a sole or decisive authority.

## 9. Data validation and build requirements

Implement scripts that: - Normalize domains to lowercase ASCII/punycode
where appropriate. - Strip schemes, paths, ports, query strings, and
leading/trailing whitespace. - Reject URLs, IP addresses, malformed
hostnames, and empty values where a domain is required. - Deduplicate
domains deterministically. - Validate category, language, country,
confidence, and review-status values. - Require evidence fields for
verified entries. - Keep `needs_review` entries out of the conservative
feed. - Keep satire and bias-only entries out of all default block
feeds. - Generate all output lists reproducibly from `data/domains.csv`
or JSON. - Sort outputs deterministically. - Include a build timestamp
or release version without making every build produce noisy changes. -
Test output syntax for each supported format.

Add GitHub Actions to run validation on every pull request and push. A
failed validation must fail the workflow.

## 10. Feed tiers

### Conservative (default recommendation)

Only high-confidence, verified entries with strong evidence of
fabricated content, repeated material deception, or confirmed
impersonation. Avoid ambiguous publishers.

### Aggressive (opt-in)

May include medium-confidence entries and broader documented
disinformation networks, but must still have evidence and clear category
metadata. Never use nationality or political viewpoint as a shortcut.

Provide category-specific feeds so users can choose to block only
impersonation, for example, without blocking every unreliable publisher.

## 11. README requirements

The public README must clearly explain: - What the project blocks and
what it does not block. - The difference between fabricated news,
unreliable reporting, propaganda, conspiracy content, and
impersonation. - Why political bias, state ownership, satire, and
nationality are not sufficient grounds for default blocking. - How to
subscribe to each feed in uBlock Origin, AdGuard, and Pi-hole. - How
often the data is reviewed and updated. - How users can report false
positives and request removals. - The evidence and licensing policy. -
That domain-level blocking is an imperfect signal and does not determine
whether every article on a site is false.

## 12. Contribution and review process

Require pull requests to include: 1. Domain(s) proposed. 2.
Classification and language. 3. Direct evidence URLs. 4. Concise
evidence summary. 5. Date checked. 6. Explanation of why the
classification is domain-wide rather than article-specific. 7. Reviewer
approval.

Create a removal process for false positives, domain ownership changes,
site redirects, expired domains, and corrected editorial practices.

Do not let automated scraping add domains directly to the conservative
list. Automation may discover candidates; human review must approve
them.

## 13. Immediate execution plan for the next AI agent

1.  Open the target GitHub repository and inspect its default branch,
    existing files, license, open issues, and recent commits.
2.  Check whether `README.md`, `handoff.md`, or equivalent files already
    exist. Preserve existing work and avoid overwriting another agent's
    changes.
3.  Commit this handoff as `handoff.md` if it is not already present.
4.  Add the schema and canonical dataset structure.
5.  Implement normalization, validation, deduplication, and
    deterministic feed generation.
6.  Add GitHub Actions validation.
7.  Research candidate domains from permitted sources, starting with
    English, Chinese, and Persian coverage.
8.  Verify each candidate independently and record evidence, dates,
    category, language, country, confidence, and provenance.
9.  Build conservative and aggressive feeds plus the requested
    format-specific outputs.
10. Run validation locally and in GitHub Actions.
11. Review a sample from every language and category for false
    positives.
12. Commit and push in small, descriptive commits.
13. Confirm the final commit SHA and provide the actual repository paths
    and raw subscription URLs.

## 14. Completion checklist

-   [ ] Repository inspected; no existing work overwritten.
-   [ ] `handoff.md` committed.
-   [ ] README explains scope, methodology, installation, licensing, and
    appeals.
-   [ ] Canonical schema and dataset exist.
-   [ ] English, Chinese, and Persian feeds exist.
-   [ ] US, China, Iran, and international metadata feeds exist.
-   [ ] Category-specific feeds exist.
-   [ ] uBlock Origin, AdGuard, Pi-hole, and hosts outputs validate.
-   [ ] Conservative and aggressive tiers are separate.
-   [ ] Satire and bias-only domains are excluded from default
    hard-block feeds.
-   [ ] Factnameh is not treated as a trusted authority; its status
    remains an unverified allegation unless independently assessed.
-   [ ] Every verified domain has evidence and provenance.
-   [ ] Licensing and attribution have been checked.
-   [ ] CI validation passes.
-   [ ] Final commit SHA and subscription URLs are reported.

**Do not optimize for the biggest raw domain count. Optimize for breadth
with defensible evidence, transparent labels, reproducible feeds, and
low false-positive risk.**
