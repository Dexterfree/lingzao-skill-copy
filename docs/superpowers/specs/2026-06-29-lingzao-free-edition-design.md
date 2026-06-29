# Lingzao Free Edition (Playbook-Only) — Design Spec

- **Date:** 2026-06-29
- **Status:** Approved (design)
- **Source repo:** `lingzao-skill-copy` (local copy, MIT-0)
- **Target skill name:** `lingzao` (unchanged; documented as free edition)
- **Baseline version:** `0.1.65`
- **Target version:** `0.1.66-free`

## 1. Problem & Goal

The Lingzao skill ships in two layers:

1. **Methodology layer** — 31 markdown playbooks + `SKILL.md`. Pure text workflows
   for self-media creators (Xiaohongshu / Douyin / WeChat official accounts):
   account diagnosis, title design, keyword design, content packaging,
   pre-publish checks, post-publish review, benchmark judgment, etc. **No network
   dependency.**
2. **Paid API layer** — `scripts/lingzao_client.py` (1732 lines) plus
   `configure.py`, `check_version.py`, `setup.sh`. A Python CLI that calls
   `https://lingzao.atian.vip`, requiring an `lgz_` API key and consuming
   credits (积分). Endpoints cover note/creator/article search, profile/post
   detail, comments, video-copy extraction, and image generation.

The goal is to produce a **free edition** that works with **zero paid-API
dependency, zero API key, zero credits, and no outbound calls to the Lingzao
backend**. The chosen route (Route B, approved) is the **playbook-only** edition:
keep the methodology brain, remove the paid data layer, and reframe data sourcing
onto the agent's own tools plus user-provided material.

### Non-goals (explicitly excluded)

- No replacement backend of any kind (Route A/C were rejected).
- No free substitute for image generation or video-transcript extraction.
  These are paid-only features and are **removed**, not faked.
- No modification of the 18 pure-methodology playbooks' core content beyond a
  final residual-reference sweep.
- No attempt to circumvent the Lingzao server's own payment/credit enforcement
  to obtain its data. (The server enforces credits server-side; the free edition
  simply does not call that server.)

## 2. Architecture After

```
lingzao/                      # skill name unchanged; docs mark free edition
├── SKILL.md                  # rewritten: methodology entry, no Commands/Setup/key
├── README.md                 # rewritten: free-edition positioning
├── VERSION                   # 0.1.66-free
├── LICENSE                   # unchanged (MIT-0)
├── playbooks/                # 13 rewritten/lightly edited + 18 swept
└── agents/
    ├── openai.yaml           # description → methodology positioning
    └── openclaw.yaml         # drop LINGZAO_API_KEY dep + paid homepage entry
# scripts/ directory removed entirely
```

Removed runtime artifacts (no longer created):

- `~/.lingzao/bin/lingzao` wrapper
- `~/.lingzao/config.json`
- Any outbound HTTP to `lingzao.atian.vip` or `assets-tian.midao.site`

The skill becomes a **static, read-only set of markdown files** plus minimal
agent metadata. Installing it = pointing an agent at the directory.

## 3. Core Reframe — Data Sourcing Rule

Wherever the original invoked the paid CLI to *fetch* data, the free edition uses
exactly one of three sources, selected by what the user actually provides. This
rule is stated once in `SKILL.md` and applied consistently across rewritten
playbooks.

1. **User-provided material (primary).** Note links, screenshots, pasted copy
   text, backend data screenshots, drafts, oral ideas. The agent reasons over
   whatever the user supplies. This is the default expectation.
2. **Agent's own web tools (secondary).** When public content is reachable, the
   agent uses its built-in `WebSearch` / `WebFetch` (or platform equivalent).
   The agent states honestly that Xiaohongshu / Douyin are heavily
   anti-scrape, so results may be partial.
3. **Honest "not supported" (explicit).** When a feature genuinely requires the
   paid backend — real-time structured Xiaohongshu/Douyin data, guaranteed
   video transcripts, image generation — the agent says so plainly instead of
   fabricating output.

**Credits / 积分 language is removed everywhere it appears.** Where a playbook
used credit tiers to express *scope* (e.g. fast/standard/batch keyword search),
the same intent is preserved as **scope tiers** driven by how much material the
user provides and how many searches the agent runs — not by credits.

## 4. Playbook Handling Matrix

13 playbooks contain dead references to the paid CLI, credits, API keys, the
wrapper, or the dashboard. Handling per file:

| File | Handling | Detail |
|---|---|---|
| `search-credit-notice.md` | **Delete** | Entire file explains credits; none exist in free edition. Its useful residue (basic-vs-deep scope, "ask before expanding") folds into the rewritten interaction map. |
| `lingzao-progressive-interaction-map.md` | **Rewrite** | Routing backbone. Replace every `~/.lingzao/bin/lingzao <cmd>` call and credit section with the data-sourcing rule (ask user for link/material, or use WebSearch). Keep the routing logic and progressive-question structure. |
| `keyword-insight-report-template.md` | **Rewrite** | Remove credit estimates and `search-notes`-lookup framing. Replace with "agent WebSearch + user-provided keyword list". Keep report structure and scoping tiers. |
| `keyword-to-publishable-content-package.md` | **Rewrite** | Convert credit tiers (快速/标准/批量) into scope tiers based on material provided + number of searches. Keep content-package structure. |
| `benchmark-account-discovery-quality-gate.md` | **Rewrite** | Replace "call `get-user-info` after credit framing" with "use WebSearch / user supplies candidate accounts". Keep the quality-gate criteria. |
| `comparable-account-breakdown-report-template.md` | **Rewrite** | Remove credit framing; keep breakdown structure and "worth learning / not copyable" methodology. |
| `self-account-diagnosis-report-template.md` | **Rewrite** | Remove its API/credit references; keep diagnosis methodology, conclusion card, action advice, human-touch closing. |
| `image-generation-execution-workflow.md` | **Demote to prompt-only** | No `generate-image` calls. Agent produces ready-to-paste image prompts/briefs for the user's own image tool, plus the visual-director quality gate and ugly/crowded/generic repair guidance. |
| `image-generation-agent-integration-guide.md` | **Trim** | Remove API-wiring, credits, web-dashboard, API-key sections. Keep model-agnostic "good-vs-bad image standards", reference-image usage, and known generation-bug guidance. |
| `post-publish-data-review-workflow.md` | **Light edit** | "Search same-stage references after confirming credits" → "use WebSearch or user-provided peers". |
| `content-knowledge-base-workflow.md` | **Light edit** | Remove credit mentions; keep knowledge-base building methodology. |
| `product-judgment-and-feedback-loop.md` | **Light edit** | "Prevents wasted credits" → rephrase without credits. |
| `retention-and-follow-up-loop.md` | **Light edit** | "Wider search consumes credits" → rephrase without credits. |

The **18 pure-methodology playbooks** keep their core content unchanged. After
the matrix above is applied, a final residual sweep runs across *all 31*
playbooks to catch any leftover `网页版` / `doctor` / `check-version` /
`~/.lingzao` / dashboard references.

The 18 clean playbooks (no core changes expected):

`atian-creator-judgment-framework.md`, `audience-persona-fit-check.md`,
`beginner-account-start-and-topic-radar.md`,
`creator-case-general-analysis-framework.md`,
`draft-rewrite-and-benchmark-workflow.md`,
`monetization-path-judgment-library.md`,
`mother-content-cross-platform-distribution.md`,
`pre-publish-readiness-check.md`, `publishing-keyword-design-check.md`,
`reference-image-graphic-note-workflow.md`,
`self-account-peer-horizontal-diagnosis.md`,
`single-note-breakdown-workflow.md`,
`track-difficulty-judgment-library.md`,
`visual-generation-and-cover-workflow.md`,
`visual-reference-style-library.md`, `xhs-operation-task-tree.md`,
`xhs-profile-bio-design.md`, `xhs-title-design-check.md`.

## 5. SKILL.md / Metadata / README

### `SKILL.md`

- **Keep:** frontmatter `name: lingzao` (description trimmed to drop paid
  actions), the "Use this skill when…" trigger list (paid actions removed),
  the playbook index (minus deleted file), the public-wording guardrails.
- **Delete:** `Commands` section (all CLI subcommands), `Setup` section
  (base-url / api-key / doctor / check-version / install wrapper), the entire
  `Install And Paid Capability Entry` section, the `Before Calling`
  parameter-asking block, the post-search time-saved footer logic.
- **Add:** a short "Data Sources" section stating the three-tier data-sourcing
  rule from §3, and a one-line note that this is the free edition.

### `agents/openclaw.yaml`

- Remove `requires.env: LINGZAO_API_KEY`, `primary_env`, the paid `homepage`,
  and the entire `requires`/`bins` block (`setup.sh`, which needed
  `python3`/`bash`, is deleted).
- Keep `display_name`, `short_description`, `emoji`.

### `agents/openai.yaml`

- Rewrite `short_description` and the `default_prompt` to drop paid
  public-content research actions and reflect methodology positioning.

### `README.md`

- Remove the install-with-base-url/api-key flow, the `lingzao doctor` /
  `check-version` flow, the "Paid Capability Entry" wording, and the update
  flow that reruns `setup.sh`.
- Reposition as: a pure methodology skill — install = point an agent at the
  directory; no key, no credits, works offline.

## 6. Verification

A change is "done" only when all hold:

1. **Zero residual references.** `grep -rn` across the repo returns nothing for:
   `lgz_`, `/api/v1`, `credits`, `积分`, `generate-image`, `~/.lingzao`,
   `search-notes`, `get-note-detail`, `get-user-info`, `get-user-posted-notes`,
   `analyze-user-profile`, `get-note-comments`, `extract-video-copy`,
   `get-article-detail`, `API Key`, `lingzao.atian.vip`, `assets-tian.midao`.
2. **`scripts/` removed**, repo still has `SKILL.md`, `playbooks/`, `agents/`.
3. **`agents/` metadata carries no required env** beyond what the free edition
   actually needs (none expected).
4. **Spot-check 3 rewritten playbooks** (interaction-map, keyword-insight,
   image-execution) for self-consistency and zero dead references.
5. **VERSION** = `0.1.66-free`; README and SKILL.md both state free edition.

## 7. Risks & Mitigations

- **Lost capability is visible.** Mitigation: every rewritten playbook states
  the data-sourcing rule and, where a feature is gone (image gen, video
  transcripts), says so explicitly rather than silently.
- **Rewrite drift.** 13 files edited by hand risk inconsistency. Mitigation:
  the single data-sourcing rule in §3 is copied verbatim where relevant; the
  verification grep is the backstop.
- **Agent confusion from stale playbook index in SKILL.md.** Mitigation: SKILL.md
  playbook index is updated to drop `search-credit-notice.md` and reflect the
  demoted image workflow.

## 8. Out of This Spec

Implementation sequencing, per-file edit order, and exact wording are deferred to
the implementation plan produced by the `writing-plans` skill.
