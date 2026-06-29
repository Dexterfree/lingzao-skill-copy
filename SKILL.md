---
name: lingzao
description: Lingzao is a free, methodology-only creator-content skill for Xiaohongshu/XHS, Douyin, and WeChat official-account creators. It routes requests to playbooks for account diagnosis, benchmark discovery, single-note breakdowns, keyword insight and content packaging, title and profile-bio design, pre-publish checks, post-publish review, cross-platform distribution, and visual/cover prompt packages. It does not call any paid API; data comes from the agent's own web tools and user-provided material.
---

# Lingzao (Free Edition)

Lingzao is a **free, methodology-only** creator-content skill. It gives an agent
the workflows for self-media operation on Xiaohongshu, Douyin, and WeChat
official accounts. It does **not** call any paid API — there is no API key, no
paid balance, and no setup script. Install by pointing an agent at this
directory.

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

Use this skill when the user asks to:

- Diagnose an account or judge a track.
- Find benchmark accounts and break down why they work.
- Break down a note the user supplied (link, screenshots, or pasted copy).
- Design titles, publishing keywords, or profile bios.
- Run a pre-publish readiness check on a finished draft.
- Review published note performance from user-provided data.
- Build a cross-platform distribution package from one topic or draft.
- Produce cover or visual prompt packages.

## Agent Playbooks

For higher-level creator strategy tasks, use the playbooks in
`<skill_root>/playbooks/` before answering. They turn Lingzao's workflows into
creator workflows instead of isolated lookups.

Use these playbooks when relevant:

- `lingzao-progressive-interaction-map.md`: route vague user inputs, homepage
  links, note links, drafts, and reference-image requests with light questions.
- `atian-creator-judgment-framework.md`: apply A Tian's account-stage,
  memory-anchor, content-mainline, and bottleneck judgment.
- `creator-case-general-analysis-framework.md`: analyze any creator case across
  tracks by identifying the account archetype, memory anchor, new narrative,
  proof system, audience desire, content engine, format engine, comment demand,
  commercial entry, hidden resources, learnable parts, non-copyable parts, and
  user-fit tests.
- `beginner-account-start-and-topic-radar.md`: handle zero-to-one creator
  questions, topic discovery, keyword trees, and low-follower viral references.
- `keyword-insight-report-template.md`: create scoped keyword insight reports
  from a main keyword plus confirmed related/dropdown terms, with clear credit
  estimates before expanding.
- `keyword-to-publishable-content-package.md`: turn a keyword or vague topic
  into publishable Xiaohongshu content packages with selected references,
  topic angles, titles, cover copy, graphic-note structure, spoken scripts,
  Vlog storyboards, body direction, and publishing keywords.
- `mother-content-cross-platform-distribution.md`: turn one topic, draft,
  note breakdown, product update, screenshot, transcript, or oral idea into a
  one-stop cross-platform distribution package. When users say "一条龙",
  "全平台同步", "分发包", or "一个模板发多个平台", start with the basic
  Xiaohongshu + Moments + WeChat public-account package, then offer optional
  expansion to podcast, X, Knowledge Planet, Bilibili, video account/Douyin,
  Xiaohongshu image package, or knowledge-base/SOP.
- `pre-publish-readiness-check.md`: before posting, ask whether the content is
  already finished and then check content clarity, image/page readiness, cover
  recognition, title clickability, first 3 lines or first 3 seconds, and natural
  keyword embedding.
- `audience-persona-fit-check.md`: before titles, keywords, account operation,
  or content-package decisions, infer or ask who the content is for, who will
  click, who will not click, and which audience/city/life-stage keywords should
  shape the output.
- `xhs-title-design-check.md`: design or diagnose Xiaohongshu titles after the
  user sends a topic, draft, cover copy, reference note, or content package;
  default to 3 strongest titles with keyword anchor and click reason instead
  of a 10-title pool.
- `xhs-profile-bio-design.md`: write or diagnose Xiaohongshu 100-character
  profile bios and homepage introductions that clarify who the account is for,
  what it shares, why to follow, and how it connects to nickname, pinned notes,
  account stage, audience keywords, city keywords, and light commercial paths.
- `benchmark-account-discovery-quality-gate.md`: find or judge benchmark
  accounts with a default quality gate: still updating, recent high-performing
  works, track/audience fit, stage fit, and clear learnable parts; stale
  accounts should be marked as historical references, not main benchmarks.
  User-facing results should show direct creator profile links and the specific
  recent high-interaction works, not raw creator IDs. The first discovery round
  should return up to 5 strong accounts, not 10-20 accounts; expand only after
  the user confirms follower range, stage, city, audience, format, or asks for
  more. Include follower count, total liked count, latest update, recent
  30-day hit works with note metrics, content format, and why each account is
  worth learning; sort visible recommendations by follower count from high to
  low when available.
- `self-account-peer-horizontal-diagnosis.md`: compare the user's own account
  with same-track, same-stage, or same-follower-range peer accounts when the
  user explicitly asks for peer comparison, such as "横向对比", "同级账号",
  "对标账号", "找 5-15w 粉账号和我比", or "和同赛道账号比我差在哪里". Generic
  own-account concerns such as "看看我现在的问题" or "我是不是说话太快" should
  stay on `self-account-diagnosis-report-template.md` unless the user also asks
  to compare against peers. It combines own-account diagnosis, active benchmark
  selection, peer-account tables, title/cover/opening/speech/content-system
  comparison, top gaps, 30-day adjustment plans, and a human next-step loop.
- `single-note-breakdown-workflow.md`: break down one Xiaohongshu/Douyin note
  link by title, cover, outline/script, shooting/editing layer when visible,
  comment demand, viral mechanism, learnable parts, non-copyable parts, and
  adaptation into the user's own graphic note, spoken script, Vlog storyboard,
  or knowledge-base card. User phrases such as "完整分析这条笔记", "深度拆解",
  "拆细一点", "拍摄手法", "分镜", or "剪辑节奏" should trigger the deeper
  breakdown instead of a short summary.
- `publishing-keyword-design-check.md`: design the final 10 Xiaohongshu
  publishing keywords for a finished draft and check whether title, cover copy,
  opening lines, and keyword field carry the keywords naturally.
- `track-difficulty-judgment-library.md`: judge common tracks such as female
  growth, career, good products, local life, health, fashion, and AI tools.
- `monetization-path-judgment-library.md`: answer whether a track or account
  can monetize through ads, courses, community, consulting, lead generation,
  products, stores, or enterprise conversion.
- `self-account-diagnosis-report-template.md`: structure own-account diagnosis
  reports, follow-up actions, and a human closing with "人情味" that turns
  sharp diagnosis into one small next experiment instead of ending at a cold
  action list. Own-account diagnosis should also include a share-worthy
  conclusion card, action advice, and psychological reassurance.
- `comparable-account-breakdown-report-template.md`: decide whether another
  account is worth learning from, what can be learned, and what cannot be copied.
- `draft-rewrite-and-benchmark-workflow.md`: rewrite drafts, adapt viral
  formulas, and review multiple content ideas without only polishing sentences.
- `reference-image-graphic-note-workflow.md`: turn reference images into
  Xiaohongshu 4-page or 7-page graphic-note packages.
- `visual-generation-and-cover-workflow.md`: route Xiaohongshu covers, graphic
  notes, WeChat image packs, no-person knowledge cards, and product/ecommerce
  visuals into ready-to-use prompt packages.
- `image-generation-execution-workflow.md`: produce ready-to-paste cover/image
  **prompt packages** (no image API), run a visual-director quality gate, and
  repair ugly/crowded/generic generations instead of leaving ordinary users
  with raw prompts.
- `image-generation-agent-integration-guide.md`: model-agnostic good-vs-bad
  image standards and reference-image usage, including stable generation
  input/output fields, known generation bugs, friendly failure handling, and
  A Tian's example-collection homework.
- `visual-reference-style-library.md`: classify A Tian's internal visual
  reference folders into travel/food covers, WeChat article images, AI-person
  infographics, Lingzao no-person knowledge cards, product conversion images,
  face-led keyword video covers, interaction prompt covers, and text-dense
  screenshot graphic notes, and room-as-identity lifestyle covers.
- `post-publish-data-review-workflow.md`: review published Xiaohongshu notes
  from note links, backend screenshots, scripts, covers, and 24h/48h/7d data.
- `content-knowledge-base-workflow.md`: turn saved notes, public creator links,
  keyword results, viral examples, and creator distillation requests into
  user-owned topic, title, cover, structure, account-reference,
  creator-research, and publishing-review libraries.
- `retention-and-follow-up-loop.md`: end useful outputs with one concrete next
  step such as published-note data review, reusable reference-search templates,
  draft feedback, or a post-diagnosis small experiment with a return loop. It
  also defines the SOP for not letting the user's words drop on the floor:
  acknowledge resistance, lower the next action, and ask one concrete
  next-step question. Dense outputs should offer Word, HTML/webpage preview, or
  knowledge-base-ready packaging instead of leaving users with a wall of chat
  text. When users say the diagnosis is accurate but they lack action, route to
  a post-diagnosis activation package instead of adding more pressure.
- `product-judgment-and-feedback-loop.md`: judge where users are really stuck,
  explain Lingzao in human language, build content/sales narratives, turn user
  feedback into product iteration, and decide which requests are worth building
  versus noise.
- `xhs-operation-task-tree.md`: route Lingzao users by concrete Xiaohongshu
  operation tasks instead of course lists, covering homepage diagnosis,
  benchmark discovery, viral-note adaptation, topic generation, content
  production, cover/image work, pre-publish checks, post-publish review,
  acquisition paths, and knowledge-base automation.

Keep public wording focused on creator-content research and workflow support.
Do not promise viral growth, guaranteed monetization, full monitoring, raw data
export, or copying another creator's content.
