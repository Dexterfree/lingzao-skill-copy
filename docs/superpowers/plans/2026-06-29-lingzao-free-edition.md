# Lingzao Free Edition (Playbook-Only) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Strip the paid API/key/credits layer from the `lingzao` skill, leaving a self-consistent, zero-dependency methodology skill that sources data from the agent's web tools + user-provided material.

**Architecture:** Delete `scripts/` and the credits playbook. Rewrite `SKILL.md`/`README.md`/agent metadata to remove all paid entry points. Apply a single data-sourcing rule across 12 playbooks that still reference the dead CLI/credits, deleting or replacing each residual line. Final repo-wide grep gate confirms zero paid-layer residue.

**Tech Stack:** Markdown skill files + YAML agent metadata. No code, no runtime, no tests-as-code — the "test" for every task is a `grep` verification gate (see Global Constraints). This plan adapts TDD to a content refactor: each task's cycle is **read file → apply listed transformations → run grep gate → commit**.

## Global Constraints

Every task implicitly includes these. Copied verbatim from the spec (`docs/superpowers/specs/2026-06-29-lingzao-free-edition-design.md`).

**GC-1 — Version:** `VERSION` file becomes `0.1.66-free` (Task 4).

**GC-2 — The residual-reference pattern (the gate).** After a task touches a file, this grep must return **zero** lines for that file:

```
lingzao |search-notes|get-note-detail|get-user-info|get-user-posted-notes|analyze-user-profile|get-note-comments|extract-video-copy|generate-image|get-article-detail|/api/v1|lgz_|API Key|api_key|积分|credits|base-url|base_url|doctor|check-version|网页版|atian.vip|assets-tian
```

Note: `lingzao ` (with trailing space) matches command calls like `lingzao search`, `~/.lingzao/bin/lingzao <cmd>`, and `Lingzao searches`. It does **not** match the standalone skill name `Lingzao`/`灵造` in prose, which is allowed.

**GC-3 — Shared Replacement Block A (data-sourcing note, Chinese).** Insert at the top of any playbook that previously drove paid lookups (the routing backbone, keyword, benchmark, account-diagnosis playbooks). Copy verbatim:

```markdown
> **数据来源说明（免费版）**：本 playbook 不调用任何付费接口。需要素材时按以下顺序获取：
> 1. **用户已提供**（笔记链接、截图、粘贴文案、后台数据、草稿）——优先基于这些判断；
> 2. **Agent 自带 WebSearch / WebFetch** 抓取公开内容（小红书/抖音强反爬，结果可能不全，需如实告知）；
> 3. 真正需要实时结构化数据 / 逐字稿 / 出图等付费能力的，直接说"免费版不支持"，不伪造。
```

**GC-4 — Shared Replacement Block B (search scope tiers, replaces credit tiers).** Where a playbook used 快速/标准/批量 or 快速洞察/标准报告/深度报告 with credit numbers, replace the tier block with scope tiers (credit-free). Copy verbatim, adapting the noun to the playbook:

```markdown
先确认范围，避免一次性铺太大：

- A. 快速版：主关键词 1 次（用户已有素材，或 1 次 WebSearch），先给方向 / 快速判断。
- B. 标准版：主关键词 + 3 个相关词（或用户多给几条参考素材），产出更完整的结构。
- C. 深度版：8–10 个以上相关词 + 代表样本深读，适合做策略。
开始前先列清楚要做哪些搜索 / 读哪些素材，用户确认后再继续，不自行扩大范围。
```

**GC-5 — Shared Replacement Block C (image "not supported, prompt-only").** For the demoted image workflow, insert verbatim:

```markdown
> **出图能力说明（免费版）**：免费版不内置出图接口。本流程的产出是"可直接粘贴到任意出图工具（Midjourney / SD / GPT-4o 等）的 prompt + 视觉 brief"，由你自行生成图片。Agent 不调用任何出图命令，也不消耗任何额度。
```

**GC-6 — Commit cadence:** one commit per task, conventional-commit messages as given.

**GC-7 — No fabrication:** never invent API responses. If a feature needs paid data, say so (Block A rule 3).

---

### Task 1: Delete the paid API layer (`scripts/`)

**Files:**
- Delete: `scripts/lingzao_client.py`, `scripts/configure.py`, `scripts/check_version.py`, `scripts/setup.sh` (the whole `scripts/` directory)

**Interfaces:** Produces — a repo with no paid client code. Later tasks assume `scripts/` is gone.

- [ ] **Step 1: Confirm the directory contents before deletion**

Run: `ls scripts/`
Expected: `check_version.py  configure.py  lingzao_client.py  setup.sh`

- [ ] **Step 2: Delete the directory**

Run: `git rm -r scripts/`

- [ ] **Step 3: Verify deletion + no repo references**

Run: `ls scripts/ 2>&1; echo "---"; grep -rn "scripts/" . --include="*.md" --include="*.yaml" --include="*.sh" 2>/dev/null | grep -v "docs/superpowers"`
Expected: `ls: scripts/: No such file or directory`, then nothing after `---` (no remaining `scripts/` references outside the spec/plan docs).

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "feat: remove paid API client layer (scripts/)"
```

---

### Task 2: Delete the credits playbook

**Files:**
- Delete: `playbooks/search-credit-notice.md`

**Interfaces:** Produces — playbook index in `SKILL.md` (Task 3) must no longer reference `search-credit-notice.md`.

- [ ] **Step 1: Delete the file**

Run: `git rm playbooks/search-credit-notice.md`

- [ ] **Step 2: Verify gone**

Run: `ls playbooks/search-credit-notice.md 2>&1`
Expected: `No such file or directory`

- [ ] **Step 3: Commit**

```bash
git commit -m "feat: remove credits-notice playbook (no credits in free edition)"
```

---

### Task 3: Rewrite `SKILL.md` (entry point — establishes the data-sourcing rule)

**Files:**
- Modify: `SKILL.md` (592 lines)

**Interfaces:** Produces — the canonical data-sourcing statement (Block A) and the cleaned playbook index that every later playbook task must stay consistent with. Consumes — Task 1/2 (scripts + credits playbook already gone).

This is the anchor file. Transformations:

- [ ] **Step 1: Read current `SKILL.md`**

Run: read `SKILL.md` in full (592 lines) so you see the sections being removed.

- [ ] **Step 2: Replace the frontmatter `description`**

Replace the long `description:` line with (drops paid actions, keeps methodology triggers):

```yaml
description: Lingzao is a free, methodology-only creator-content skill for Xiaohongshu/XHS, Douyin, and WeChat official-account creators. It routes requests to playbooks for account diagnosis, benchmark discovery, single-note breakdowns, keyword insight and content packaging, title and profile-bio design, pre-publish checks, post-publish review, cross-platform distribution, and visual/cover prompt packages. It does not call any paid API; data comes from the agent's own web tools and user-provided material.
```

- [ ] **Step 3: Trim the "Use this skill when" trigger list**

Remove every bullet that describes a paid lookup (search notes, search suggestions, search creators, get creator profile, recent posts, analyze profile, post detail, post comments, article detail/stats/related, extract video copy, generate image). Keep methodology triggers (diagnose account, find benchmarks, break down a note the user supplied, design titles/keywords/bios, pre-publish check, post-publish review, distribution package, cover/visual prompt package).

- [ ] **Step 4: Delete these sections entirely**

Delete: the entire `## Agent Playbooks` bullet that references `search-credit-notice.md` (the single bullet only, keep the rest of the playbook index), the whole `## Install And Paid Capability Entry` section, the whole `## Setup` section, the whole `## Before Calling` section, and the whole `## Commands` section (every `### Search Notes` … `### Generate Image` subsection). Also delete the `## Setup`-adjacent update/version paragraphs.

- [ ] **Step 5: Add the free-edition header + data-sourcing section**

At the top of the body (after frontmatter, before the first `##`), insert:

```markdown
# Lingzao (Free Edition)

Lingzao is a **free, methodology-only** creator-content skill. It gives an agent
the workflows for self-media operation on Xiaohongshu, Douyin, and WeChat
official accounts. It does **not** call any paid API and needs **no API key, no
credits, no setup script** — install by pointing an agent at this directory.

## Data Sources

This skill does not fetch data from a paid backend. When a workflow needs input,
use these sources in order:

1. **User-provided material** (primary): note links, screenshots, pasted copy,
   backend data, drafts. Reason over whatever the user supplies.
2. **The agent's own web tools** (secondary): `WebSearch` / `WebFetch` for public
   content that is reachable. State honestly that Xiaohongshu / Douyin are
   heavily anti-scrape, so results may be partial.
3. **Honest "not supported"**: features that need a paid backend (real-time
   structured Xiaohongshu/Douyin data, guaranteed video transcripts, image
   generation) are stated plainly as unsupported rather than fabricated.
```

- [ ] **Step 6: Clean the playbook index**

In `## Agent Playbooks`, remove the `search-credit-notice.md` bullet (file deleted in Task 2). Rephrase the `image-generation-execution-workflow.md` bullet to "produce ready-to-paste cover/image **prompt packages** (no image API)". Rephrase the `image-generation-agent-integration-guide.md` bullet to "model-agnostic good-vs-bad image standards and reference-image usage". Leave all other playbook bullets' methodology descriptions intact (they remain valid).

- [ ] **Step 7: Gate**

Run: `grep -nE "lingzao |search-notes|get-note-detail|get-user-info|get-user-posted-notes|analyze-user-profile|get-note-comments|extract-video-copy|generate-image|get-article-detail|/api/v1|lgz_|API Key|api_key|积分|credits|base-url|base_url|doctor|check-version|网页版|atian.vip|assets-tian" SKILL.md`
Expected: no output. (The standalone words `Lingzao` / `灵造` in prose are fine and excluded by the pattern.)

- [ ] **Step 8: Commit**

```bash
git add SKILL.md
git commit -m "feat: rewrite SKILL.md as free methodology entry point"
```

---

### Task 4: Reposition metadata (`VERSION`, `README.md`, `agents/*.yaml`)

**Files:**
- Modify: `VERSION`
- Modify: `README.md`
- Modify: `agents/openai.yaml`
- Modify: `agents/openclaw.yaml`

**Interfaces:** Produces — version `0.1.66-free`, free-edition README, env-free agent metadata.

- [ ] **Step 1: Bump VERSION**

Replace the contents of `VERSION` with exactly:

```
0.1.66-free
```

- [ ] **Step 2: Rewrite `agents/openclaw.yaml`**

Replace the entire file with:

```yaml
interface:
  display_name: "灵造"
  short_description: "免费创作者内容方法论 Skill（小红书 / 抖音 / 公众号）"
  emoji: "🔎"
```

(Drops `homepage` paid entry and the whole `requires`/`bins`/`primary_env` block — `setup.sh` is gone.)

- [ ] **Step 3: Rewrite `agents/openai.yaml`**

Replace `short_description` and `default_prompt` so they drop paid public-content-research actions. Use:

```yaml
interface:
  display_name: "Lingzao"
  short_description: "Free creator-content methodology skill"
  default_prompt: "Use $lingzao for self-media creator workflows: account diagnosis, benchmark discovery, single-note breakdowns (from links/screenshots/copy the user provides), keyword insight, keyword-to-content packages, cross-platform distribution packages, Xiaohongshu title and profile-bio design, audience persona fit, pre-publish readiness checks, publishing keyword design, draft rewrites, reference-image graphic notes, cover/visual prompt packages, post-publish data review, Word/HTML/knowledge-base packaging, monetization and track judgment, and follow-up loops. Read the relevant playbook under <skill_root>/playbooks before answering. Lingzao is free and methodology-only: source data from the user's material or web search; it calls no paid API."
policy:
  allow_implicit_invocation: true
```

- [ ] **Step 4: Rewrite `README.md`**

Replace the entire file with a free-edition README. Required content: one-line positioning ("free, methodology-only creator-content skill"), the "what it helps with" list (keep the existing methodology bullets from the current README: 小红书/抖音/公众号 content research workflows, benchmark discovery, single-note breakdown, account diagnosis, keyword/title/cover work, distribution packages, post-publish review — **remove** the "封面/图文/海报等图片生成" image-generation bullet since generation is gone; keep only prompt-package framing), an install section that says "point an agent at this directory — no key, no credits, no setup script", the directory map (omit `scripts/`), and the MIT-0 license note. Remove all `setup.sh`/`--base-url`/`--api-key`/`doctor`/`check-version`/`git pull + rerun setup` flows.

- [ ] **Step 5: Gate**

Run: `grep -rnE "lingzao |search-notes|get-note-detail|get-user-info|get-user-posted-notes|analyze-user-profile|get-note-comments|extract-video-copy|generate-image|get-article-detail|/api/v1|lgz_|API Key|api_key|积分|credits|base-url|base_url|doctor|check-version|网页版|atian.vip|assets-tian|LINGZAO_API_KEY|scripts/" VERSION README.md agents/`
Expected: no output.

- [ ] **Step 6: Commit**

```bash
git add VERSION README.md agents/openai.yaml agents/openclaw.yaml
git commit -m "feat: reposition VERSION/README/agent metadata for free edition"
```

---

### Task 5: Rewrite routing backbone `playbooks/lingzao-progressive-interaction-map.md`

**Files:**
- Modify: `playbooks/lingzao-progressive-interaction-map.md` (836 lines, 17 residual hits)

**Interfaces:** Consumes — Block A/B (GC-3/GC-4). Produces — the canonical routing the other playbooks cross-reference.

- [ ] **Step 1: Read the file** in full.

- [ ] **Step 2: Insert Block A (GC-3)** at the top of the body.

- [ ] **Step 3: Apply these exact transformations** (line → action):

| Line | Current text (abbreviated) | Action |
|---|---|---|
| 104–105 | `...If the next action needs \`get-note-detail\`, ask one light question before spending credits:` | Replace with: `If the next action needs a single-note breakdown, ask one light question first — confirm the note URL/type and whether the user can paste the note or a screenshot. Do not call any paid lookup.` |
| 113 | `~/.lingzao/bin/lingzao get-user-posted-notes --url "https://<short link>"` | Replace with: `Ask the user to paste the creator's homepage link or recent-post screenshots; use WebSearch/WebFetch if the page is reachable. Do not call a CLI.` |
| 115 | `Do not call \`get-note-detail\` for that short link first.` | Replace with: `Do not assume single-note detail; confirm what the user wants first.` |
| 134 | `如果接下来要开始搜索，我会先让你选择基础搜索还是深度搜索，并告诉你两种分别能得到什么、大概会消耗多少积分。` | Replace with: `如果接下来要开始搜索，我会先让你选择基础还是深度，并告诉你两种分别能得到什么、要读哪些素材或做几次 WebSearch。` |
| 164 | `verification may spend more credits. Benchmark outputs should list follower` | Replace `verification may spend more credits.` → `verification may need more searches.` |
| 471 | `灵造不是按"你给 Agent 发了一条指令"计积分…属于深度搜索。` | Replace the whole sentence with: `灵造按实际查看的账号、笔记和内容深度划分基础与深度：基础看标题、封面、点赞/收藏/评论、链接和普通搜索能返回的文案信息；进一步看完整文案正文、字幕、逐字稿或更深结构属于深度。免费版的"查看"来自用户素材或 WebSearch，不消耗额度。` |
| 475–478 | the four `credits/页` / `credits/次` bullet lines | Delete these four bullets entirely (credits pricing table). |
| 480 | `Do not tell users that search results cost 20 credits per returned note…` | Replace with: `搜索一次可能返回多条结果。扩大范围来自更多搜索、打开详情/评论、主页深度解析、逐字稿等，开始前先和用户确认范围，不自行扩大。` |
| 492 | `A. 基础搜索：…通常是 20 credits/次…` | Replace the credit clause: remove `通常是 20 credits/次。` Keep the rest of the description. |
| 494 | `B. 深度搜索：…主页深度解析是 20 条作品 50 credits…` | Replace the credit clause: remove `是 20 条作品 50 credits、40 条作品 100 credits；评论区按页查看；短视频文案按时长计算。` Replace with `会进一步看完整文案、字幕、逐字稿和更深结构。` |
| 496 | `…Do not let the Agent spend hundreds or thousands of credits by expanding the scope on its own.` | Replace with: `Wait for the user's A/B choice before a deep pass. Do not let the Agent expand scope (more searches / more notes) on its own.` |
| 514 | `如果需要新对标、评论区、完整内容包或更深复盘，先说明积分范围` | Replace `先说明积分范围` → `先说明要做哪些搜索 / 读哪些素材`. |
| 573 | `…may require more Lingzao searches/credits because it opens more notes…` | Replace `searches/credits` → `searches`. |

- [ ] **Step 4: Gate**

Run: `grep -nE "lingzao |search-notes|get-note-detail|get-user-info|get-user-posted-notes|analyze-user-profile|get-note-comments|extract-video-copy|generate-image|/api/v1|lgz_|积分|credits|atian.vip" playbooks/lingzao-progressive-interaction-map.md`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add playbooks/lingzao-progressive-interaction-map.md
git commit -m "feat: rewrite interaction-map for free data-sourcing"
```

---

### Task 6: Rewrite `playbooks/keyword-insight-report-template.md`

**Files:**
- Modify: `playbooks/keyword-insight-report-template.md` (354 lines, 18 residual hits — the credit-estimate sections)

**Interfaces:** Consumes — Block A (GC-3), Block B (GC-4).

- [ ] **Step 1: Read the file** in full.

- [ ] **Step 2: Insert Block A (GC-3)** at top of body.

- [ ] **Step 3: Apply exact transformations:**

| Line | Action |
|---|---|
| 36 | `Lingzao Skill currently exposes \`search-notes\` for keyword searches. If an` → Replace with: `For keyword searches, use WebSearch plus any keyword list the user provides. If an` (keep the rest of the sentence intact). |
| 46 | `Each confirmed keyword that is actually searched is one \`search-notes\` lookup.` → Replace with: `Each confirmed keyword is one search (WebSearch or user-provided result).` |
| 58 | The whole `先提醒一下积分：…约 120 credits…才会继续增加。` line → Replace with Block B (GC-4) tier block. |
| 65–67 | The three `A./B./C. …credits` lines → Replace with Block B (GC-4) tier block (these are the same tier set; merge 58+65–67 into one Block B insert, deleting the originals). |
| 75 | `Use when the user wants a first look or is sensitive to credits.` → Replace with: `Use when the user wants a first look or wants to keep the search small.` |
| 91, 117, 147 | Each `Estimated credits:` header → Replace with `Estimated scope:` |
| 93 | `- 4 \`search-notes\` lookups x 20 credits = about 80 credits` → Replace with: `- ~4 keyword searches (main + 3 related)` |
| 119–121 | The three `…credits` / `extra credits` / `180-420 credits` lines → Replace with: `- 9–11 keyword searches\n- optional 5–10 single-note reads\n- scope grows if more notes/details are opened` |
| 149–152 | The four `…credits` / `extra credits` lines → Replace with: `- 16–31 keyword searches\n- 10–30 single-note reads\n- 5–10 comment reads\n- scope grows with details and comments` |

- [ ] **Step 4: Gate**

Run: `grep -nE "search-notes|get-note-detail|/api/v1|lgz_|积分|credits" playbooks/keyword-insight-report-template.md`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add playbooks/keyword-insight-report-template.md
git commit -m "feat: rewrite keyword-insight report to credit-free scope tiers"
```

---

### Task 7: Rewrite `playbooks/keyword-to-publishable-content-package.md`

**Files:**
- Modify: `playbooks/keyword-to-publishable-content-package.md` (891 lines, 5 residual hits)

**Interfaces:** Consumes — Block A (GC-3), Block B (GC-4).

- [ ] **Step 1: Read the file** in full (large — read in two passes if needed).

- [ ] **Step 2: Insert Block A (GC-3)** at top of body.

- [ ] **Step 3: Apply exact transformations:**

| Line | Action |
|---|---|
| 145 | `先确认范围，避免一下子花太多积分：` → `先确认范围，避免一次性铺太大：` |
| 147–149 | The three `A./B./C. …credits` 快速/标准/批量 lines → Replace with Block B (GC-4) tier block (delete the three originals, insert Block B). |
| 196 | `credits. Group them by intent when possible, such as audience, pain, result,` → Replace the leading `credits.` with `searches.` so it reads `searches. Group them by intent...` (keep the rest). |

- [ ] **Step 4: Gate**

Run: `grep -nE "search-notes|get-note-detail|/api/v1|lgz_|积分|credits" playbooks/keyword-to-publishable-content-package.md`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add playbooks/keyword-to-publishable-content-package.md
git commit -m "feat: rewrite keyword-to-content package to credit-free scope tiers"
```

---

### Task 8: Rewrite `playbooks/benchmark-account-discovery-quality-gate.md`

**Files:**
- Modify: `playbooks/benchmark-account-discovery-quality-gate.md` (370 lines, 6 residual hits)

**Interfaces:** Consumes — Block A (GC-3).

- [ ] **Step 1: Read the file** in full.

- [ ] **Step 2: Insert Block A (GC-3)** at top of body.

- [ ] **Step 3: Apply exact transformations:**

| Line | Action |
|---|---|
| 101 | `either call \`get-user-info\` for the selected candidates after credit framing, or` → Replace with: `either look up the selected candidates via WebSearch / user-provided profile links, or` |
| 117 | `search and may spend more credits.` → Replace with: `search and may need more lookups.` |
| 123 | `before continuing the search instead of spending credits on broad discovery.` → Replace with: `before continuing instead of running a broad discovery pass.` |
| 349 | `…这样会比一次性给你 10-20 个更省积分，也更容易找到真正能模仿的账号。` → Replace `更省积分` → `更聚焦`. |
| 366 | `- Do not hide that additional account verification can add searches/credits.` → Replace `searches/credits` → `searches`. |
| 367 | `- Do not spend credits on a broad follower-range search before the user has` → Replace with: `- Do not run a broad follower-range search before the user has` |

- [ ] **Step 4: Gate**

Run: `grep -nE "get-user-info|search-notes|/api/v1|lgz_|积分|credits" playbooks/benchmark-account-discovery-quality-gate.md`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add playbooks/benchmark-account-discovery-quality-gate.md
git commit -m "feat: rewrite benchmark-discovery to web-search/user-material sourcing"
```

---

### Task 9: Rewrite `playbooks/comparable-account-breakdown-report-template.md`

**Files:**
- Modify: `playbooks/comparable-account-breakdown-report-template.md` (284 lines, 2 residual hits)

**Interfaces:** Consumes — Block A (GC-3).

- [ ] **Step 1: Read the file** in full.

- [ ] **Step 2: Insert Block A (GC-3)** at top of body.

- [ ] **Step 3: Apply exact transformations:**

| Line | Action |
|---|---|
| 32 | `…may require more Lingzao searches/credits because it needs to inspect more notes…` → Replace `searches/credits` → `searches`. |
| 38 | The whole `积分上也先说清楚：轻量看一个主页近期内容…20 条作品是 50 credits，40 条作品是 100 credits…不会直接替你扩大搜索。` line → Replace with: `范围上也先说清楚：轻量看一个主页近期内容属于基础查看；正式报告需要读更多笔记、甚至完整正文或评论区，范围更大。开始前我会先确认范围，不会直接替你扩大搜索。` |

- [ ] **Step 4: Gate**

Run: `grep -nE "search-notes|get-note-detail|/api/v1|lgz_|积分|credits" playbooks/comparable-account-breakdown-report-template.md`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add playbooks/comparable-account-breakdown-report-template.md
git commit -m "feat: rewrite comparable-account breakdown to credit-free framing"
```

---

### Task 10: Rewrite `playbooks/self-account-diagnosis-report-template.md`

**Files:**
- Modify: `playbooks/self-account-diagnosis-report-template.md` (573 lines, 2 residual hits)

**Interfaces:** Consumes — Block A (GC-3).

- [ ] **Step 1: Read the file** in full.

- [ ] **Step 2: Insert Block A (GC-3)** at top of body.

- [ ] **Step 3: Apply exact transformations:**

| Line | Action |
|---|---|
| 127 | `2. Deep activation: may consume additional credits if it needs to find new` → Replace `consume additional credits` → `need additional searches`. |
| 314 | `\`search-credit-notice.md\` 说明是否会新增积分消耗` → Replace with: `说明这次会做哪些搜索 / 读哪些素材，先和用户确认范围`. (Also: anywhere else in the file that links to the deleted `search-credit-notice.md`, replace with the scope-confirmation wording.) |

- [ ] **Step 4: Gate**

Run: `grep -nE "search-credit-notice|search-notes|get-note-detail|/api/v1|lgz_|积分|credits" playbooks/self-account-diagnosis-report-template.md`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add playbooks/self-account-diagnosis-report-template.md
git commit -m "feat: rewrite self-account diagnosis to credit-free framing"
```

---

### Task 11: Demote image workflows to prompt-only

**Files:**
- Modify: `playbooks/image-generation-execution-workflow.md` (326 lines, 5 residual hits)
- Modify: `playbooks/image-generation-agent-integration-guide.md` (558 lines, 4 residual hits)

**Interfaces:** Consumes — Block C (GC-5). Produces — two prompt-only image playbooks (no `generate-image` calls anywhere).

- [ ] **Step 1: Read both files** in full.

- [ ] **Step 2: In `image-generation-execution-workflow.md`** — insert Block C (GC-5) at top of body. Apply exact transformations:

| Line | Action |
|---|---|
| 85–86 | `card? Do not call \`generate-image\` until the brief has at least topic + platform or format + visual direction/reference/color. This prevents spending credits on` → Replace with: `card? Do not finalize the prompt package until the brief has at least topic + platform or format + visual direction/reference/color. This prevents producing a weak brief.` |
| 153 | `If the user only approved one image, do not call \`generate-image\` again just` → Replace with: `If the user only approved one image, do not produce another prompt package just` |
| 177 | `…如果你确认继续生成，我会按这版返修 brief 再走一次生图；这会消耗一张图的积分。` → Replace with: `…如果你确认继续，我会按这版返修 brief，再给一份新的出图 prompt 让你去自己的出图工具里生成。` |
| 326 | `explicit confirmation before spending credits on another generation.` → Replace with: `explicit confirmation before producing another prompt package.` |

Throughout: replace any remaining "生成图片/出图" action by the agent with "产出出图 prompt + 视觉 brief，由用户自行生成". Keep the visual-director quality gate and the ugly/crowded/generic repair guidance intact.

- [ ] **Step 3: In `image-generation-agent-integration-guide.md`** — insert Block C (GC-5) at top of body. Apply exact transformations:

| Line | Action |
|---|---|
| 23 | `- command/tool name: \`generate-image\` or equivalent` → Replace with: `- output: a ready-to-paste image prompt + visual brief (no image API call)` |
| 491 | `- missing API key/credits` → Replace with: `- missing brief inputs (topic/platform/visual direction)` |
| 497 | `- tell the user how to open Lingzao web setup/recharge/API Key entry` → Replace with: `- tell the user this is prompt-only; they generate the image in their own tool` |
| 498 | `- if a generation job fails before producing an image, clarify whether credits` → Replace with: `- if the user's own image tool fails, help them refine the prompt rather than retrying blindly` |

Remove any other sections that wire up a `generate-image` tool/API key/recharge flow. Keep the model-agnostic "good-vs-bad image standards", "reference-image usage", and "known generation bugs" guidance.

- [ ] **Step 4: Gate (both files)**

Run: `grep -nE "generate-image|extract-video-copy|/api/v1|lgz_|API Key|api_key|积分|credits|网页版|atian.vip" playbooks/image-generation-execution-workflow.md playbooks/image-generation-agent-integration-guide.md`
Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add playbooks/image-generation-execution-workflow.md playbooks/image-generation-agent-integration-guide.md
git commit -m "feat: demote image workflows to prompt-only (no image API)"
```

---

### Task 12: Light edits — 4 low-residue playbooks

**Files:**
- Modify: `playbooks/post-publish-data-review-workflow.md`
- Modify: `playbooks/content-knowledge-base-workflow.md`
- Modify: `playbooks/product-judgment-and-feedback-loop.md`
- Modify: `playbooks/retention-and-follow-up-loop.md`

**Interfaces:** None new. These have 1–few residual lines each; rephrase to drop credits/credits-API language without changing methodology.

- [ ] **Step 1: Read all four files.**

- [ ] **Step 2: `post-publish-data-review-workflow.md`** — transform:

| Line | Action |
|---|---|
| 84 | `…or search same-stage references after confirming credits |` → Replace `after confirming credits` → `after confirming scope`. |
| 179 | `- optionally search same-stage references after confirming credits` → Replace `after confirming credits` → `after confirming scope`. |

- [ ] **Step 3: `content-knowledge-base-workflow.md`** — transform (3 hits):

| Line | Action |
|---|---|
| 153 | `think Lingzao randomly picked posts or spent credits on an opaque search.` → Replace `or spent credits on an opaque search` → `or ran an opaque search`. |
| 177 | `Use for first-pass learning or when credits should be controlled.` → Replace `when credits should be controlled` → `when search scope should be kept small`. |
| 320 | `scope before spending more credits.` → Replace `spending more credits` → `expanding scope`. |

- [ ] **Step 4: `product-judgment-and-feedback-loop.md`** — transform:

| Line | Action |
|---|---|
| 153 | `- reduces user confusion or prevents wasted credits` → Replace `prevents wasted credits` → `prevents wasted searches`. |

- [ ] **Step 5: `retention-and-follow-up-loop.md`** — transform:

| Line | Action |
|---|---|
| 168 | `- remind that wider search consumes credits and should be confirmed first` → Replace `consumes credits` → `expands scope`. |

- [ ] **Step 6: Gate (all four)**

Run: `grep -nE "search-notes|get-note-detail|/api/v1|lgz_|积分|credits" playbooks/post-publish-data-review-workflow.md playbooks/content-knowledge-base-workflow.md playbooks/product-judgment-and-feedback-loop.md playbooks/retention-and-follow-up-loop.md`
Expected: no output.

- [ ] **Step 7: Commit**

```bash
git add playbooks/post-publish-data-review-workflow.md playbooks/content-knowledge-base-workflow.md playbooks/product-judgment-and-feedback-loop.md playbooks/retention-and-follow-up-loop.md
git commit -m "feat: drop credit language from 4 light-edit playbooks"
```

---

### Task 13: Final repo-wide verification sweep

**Files:**
- Verify only: all 31 remaining playbooks + `SKILL.md` + `README.md` + `agents/` + `VERSION`.

**Interfaces:** Consumes — Tasks 1–12. Produces — the zero-residue guarantee from spec §6.

- [ ] **Step 1: Sweep the 18 "clean" playbooks for any missed residue**

Run:
```bash
grep -rnE "lingzao |search-notes|get-note-detail|get-user-info|get-user-posted-notes|analyze-user-profile|get-note-comments|extract-video-copy|generate-image|get-article-detail|/api/v1|lgz_|API Key|api_key|积分|credits|base-url|base_url|doctor|check-version|网页版|atian.vip|assets-tian" \
  playbooks/atian-creator-judgment-framework.md playbooks/audience-persona-fit-check.md playbooks/beginner-account-start-and-topic-radar.md playbooks/creator-case-general-analysis-framework.md playbooks/draft-rewrite-and-benchmark-workflow.md playbooks/monetization-path-judgment-library.md playbooks/mother-content-cross-platform-distribution.md playbooks/pre-publish-readiness-check.md playbooks/publishing-keyword-design-check.md playbooks/reference-image-graphic-note-workflow.md playbooks/self-account-peer-horizontal-diagnosis.md playbooks/single-note-breakdown-workflow.md playbooks/track-difficulty-judgment-library.md playbooks/visual-generation-and-cover-workflow.md playbooks/visual-reference-style-library.md playbooks/xhs-operation-task-tree.md playbooks/xhs-profile-bio-design.md playbooks/xhs-title-design-check.md
```
Expected: no output. If any line appears, rephrase it per the matching rule above (drop credits/CLI/dashboard language) and re-run until clean.

- [ ] **Step 2: Repo-wide final gate**

Run:
```bash
grep -rnE "lingzao |search-notes|get-note-detail|get-user-info|get-user-posted-notes|analyze-user-profile|get-note-comments|extract-video-copy|generate-image|get-article-detail|/api/v1|lgz_|API Key|api_key|积分|credits|base-url|base_url|doctor|check-version|网页版|atian.vip|assets-tian|LINGZAO_API_KEY" . --include="*.md" --include="*.yaml" --include="*.py" --include="*.sh" 2>/dev/null | grep -v "docs/superpowers/"
```
Expected: no output (the only matches allowed are inside `docs/superpowers/` spec/plan, which are excluded).

- [ ] **Step 3: Structural sanity**

Run:
```bash
echo "== scripts gone =="; ls scripts/ 2>&1
echo "== playbooks count =="; ls playbooks/*.md | wc -l
echo "== VERSION =="; cat VERSION
echo "== required env anywhere? =="; grep -rn "LINGZAO_API_KEY\|requires:" agents/ 2>/dev/null
```
Expected: `scripts/` → No such file; playbooks count → `30` (was 31, minus deleted `search-credit-notice.md`); VERSION → `0.1.66-free`; no `LINGZAO_API_KEY` / `requires:` lines.

- [ ] **Step 4: Spot-check 3 rewritten playbooks**

Open and read `playbooks/lingzao-progressive-interaction-map.md`, `playbooks/keyword-insight-report-template.md`, `playbooks/image-generation-execution-workflow.md`. Confirm: each opens with its data-sourcing/image block, has no dead CLI call, reads as self-consistent methodology.

- [ ] **Step 5: If any fixups were made in Steps 1–4, commit them**

```bash
git add -A
git commit -m "chore: final zero-residue sweep for free edition"
```
(If nothing changed, skip — the repo is already clean from Tasks 1–12.)

---

## Self-Review (run after writing — already done)

- **Spec coverage:** Every spec section maps to a task — §1 goal (all), §2 architecture (Task 1 deletes scripts, Task 4 metadata), §3 data rule (GC-3/A + Task 3), §4 matrix (Tasks 5–12, one per row; `search-credit-notice` delete = Task 2), §5 SKILL/metadata/README (Tasks 3–4), §6 verification (GC-2 per task + Task 13 full gate). No gaps.
- **Placeholder scan:** No TBD/TODO/"add error handling". Every step has concrete commands or verbatim replacement text (Blocks A/B/C are verbatim in Global Constraints; per-line tables are verbatim).
- **Type/name consistency:** Block names (A/B/C) referenced in Tasks 5–11 match Global Constraints GC-3/GC-4/GC-5 exactly. `VERSION` value `0.1.66-free` consistent across GC-1, Task 4, Task 13. Playbook count 30 in Task 13 = 31 − 1 (Task 2).
