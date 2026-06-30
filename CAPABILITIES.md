# 灵造 Skill 能力说明（免费版）

灵造是一个**免费、纯方法论**的创作者内容 Skill，给 AI Agent（Claude Code / Codex 等）用于小红书 / 抖音 / 公众号的自媒体运营。

**不调用任何付费接口、不需要 API Key、不扣积分**——装上即用。安装方式：把仓库指向 Agent 的 Skill 目录即可（详见 [README.md](README.md)）。

---

## 数据来源（免费版核心规则）

这个 Skill 不从付费后端取数据。当工作流需要素材时，按以下三档顺序获取：

1. **用户已提供**（首选）：笔记链接、截图、粘贴文案、后台数据、草稿——优先基于这些判断。
2. **Agent 自带 WebSearch / WebFetch**（次选）：抓取公开可达的内容。小红书 / 抖音强反爬，结果可能不全，需如实告知。
3. **诚实说「不支持」**：真正需要付费后端的能力（实时结构化数据、保证拿到逐字稿、直接出图），直接说明，不伪造。

---

## 能力清单（按功能分类）

### 一、入口 & 路由

| 文件 | 能做什么 |
|---|---|
| [SKILL.md](SKILL.md) | 总入口。声明免费版定位 + 数据来源三档规则，按请求路由到对应 playbook |
| [lingzao-progressive-interaction-map.md](playbooks/lingzao-progressive-interaction-map.md) | 把模糊输入 / 主页链接 / 笔记链接 / 草稿 / 参考图，用最少的追问分层路由到正确工作流 |

### 二、找方向 & 选题

| 文件 | 能做什么 |
|---|---|
| [beginner-account-start-and-topic-radar.md](playbooks/beginner-account-start-and-topic-radar.md) | 0→1 冷启动、选题发现、关键词树、低粉爆款参考 |
| [keyword-insight-report-template.md](playbooks/keyword-insight-report-template.md) | 关键词洞察报告（主词 + 相关词，按范围分快速 / 标准 / 深度档） |
| [keyword-to-publishable-content-package.md](playbooks/keyword-to-publishable-content-package.md) | 关键词或模糊话题 → 可发布的小红书内容包（标题 / 封面文案 / 图文结构 / 口播 / Vlog 分镜 / 正文 / 发布关键词） |
| [track-difficulty-judgment-library.md](playbooks/track-difficulty-judgment-library.md) | 判断赛道难度（女性成长 / 职场 / 好物 / 本地生活 / 健康 / 时尚 / AI 工具） |
| [monetization-path-judgment-library.md](playbooks/monetization-path-judgment-library.md) | 判断赛道 / 账号能否变现、走哪条路（广告 / 课程 / 社群 / 咨询 / 线索 / 产品 / 店铺 / 企业） |

### 三、账号研究 & 对标

| 文件 | 能做什么 |
|---|---|
| [atian-creator-judgment-framework.md](playbooks/atian-creator-judgment-framework.md) | A Tian 的判断框架：账号阶段 / 记忆锚点 / 内容主线 / 瓶颈 |
| [creator-case-general-analysis-framework.md](playbooks/creator-case-general-analysis-framework.md) | 通用创作者案例拆解（人设原型 / 记忆点 / 证明体系 / 受众欲望 / 内容引擎 / 可学 vs 不可抄） |
| [benchmark-account-discovery-quality-gate.md](playbooks/benchmark-account-discovery-quality-gate.md) | 找 / 评判对标账号，自带质量门（在更新 / 有爆款 / 赛道匹配 / 阶段匹配），默认一轮最多 5 个 |
| [comparable-account-breakdown-report-template.md](playbooks/comparable-account-breakdown-report-template.md) | 判断某个账号值不值得学、能学什么、不能照抄什么 |
| [self-account-diagnosis-report-template.md](playbooks/self-account-diagnosis-report-template.md) | 自己账号诊断报告（含结论卡 / 行动建议 / 人情味收尾，把尖锐诊断变成一个小实验） |
| [self-account-peer-horizontal-diagnosis.md](playbooks/self-account-peer-horizontal-diagnosis.md) | 自己 vs 同级 / 同赛道账号横向对比（说「横向对比」「同级账号」「找 5-15w 粉账号和我比」时触发） |

### 四、单条笔记拆解

| 文件 | 能做什么 |
|---|---|
| [single-note-breakdown-workflow.md](playbooks/single-note-breakdown-workflow.md) | 单条小红书 / 抖音笔记深拆（标题 / 封面 / 大纲脚本 / 拍摄剪辑 / 评论区需求 / 爆点机制 / 可学不可学 / 改成你自己的版本）。说「完整拆」「深度拆解」「拍摄手法」「分镜」触发更深一层 |

### 五、标题 / 人设 / 受众

| 文件 | 能做什么 |
|---|---|
| [audience-persona-fit-check.md](playbooks/audience-persona-fit-check.md) | 受众画像判断：这条给谁看、谁会点、谁大概率不会点 |
| [xhs-title-design-check.md](playbooks/xhs-title-design-check.md) | 小红书标题设计 / 诊断，默认给 3 个最强标题（关键词锚点 + 点击理由），不堆 10 个 |
| [xhs-profile-bio-design.md](playbooks/xhs-profile-bio-design.md) | 小红书 100 字简介 + 主页定位设计（给谁看 / 分享什么 / 为什么关注） |

### 六、内容创作 & 分发

| 文件 | 能做什么 |
|---|---|
| [draft-rewrite-and-benchmark-workflow.md](playbooks/draft-rewrite-and-benchmark-workflow.md) | 草稿改写 / 套爆款公式 / 多条内容点子复盘（不止于把句子改顺） |
| [mother-content-cross-platform-distribution.md](playbooks/mother-content-cross-platform-distribution.md) | 「一条龙」跨平台分发包：1 个选题 → 小红书 + 朋友圈 + 公众号基础包，可扩展播客 / X / 知识星球 / B 站 / 视频号 / 知识库 SOP |
| [reference-image-graphic-note-workflow.md](playbooks/reference-image-graphic-note-workflow.md) | 参考图 → 小红书 4 页或 7 页图文包（页面文案 + 版式 + 图片 prompt） |

### 七、视觉 / 封面 / 出图（prompt-only，不出图）

| 文件 | 能做什么 |
|---|---|
| [visual-generation-and-cover-workflow.md](playbooks/visual-generation-and-cover-workflow.md) | 把封面 / 图文 / 公众号配图 / 无人知识卡 / 产品视觉路由成「可直接用的 prompt 包」 |
| [image-generation-execution-workflow.md](playbooks/image-generation-execution-workflow.md) | 产出可粘贴到 Midjourney / SD / GPT-4o 的出图 prompt + 视觉 brief，带视觉导演质检和「丑图返修」 |
| [image-generation-agent-integration-guide.md](playbooks/image-generation-agent-integration-guide.md) | 好图 vs 烂图标准、参考图用法、已知生成坑、友好失败处理（**不调任何出图 API**） |
| [visual-reference-style-library.md](playbooks/visual-reference-style-library.md) | A Tian 视觉风格库分类（旅行美食 / 公众号 / AI 人物 / 知识卡 / 产品 / 人脸 / 互动 / 截图图文 / 房间人设） |

### 八、发布 & 复盘

| 文件 | 能做什么 |
|---|---|
| [pre-publish-readiness-check.md](playbooks/pre-publish-readiness-check.md) | 发布前就绪检查（内容清晰度 / 图页齐不齐 / 封面识别度 / 标题点击力 / 前 3 行 / 关键词自然度） |
| [publishing-keyword-design-check.md](playbooks/publishing-keyword-design-check.md) | 给成品定最终 10 个发布关键词，并检查标题 / 封面 / 开头 / 关键词位有没有自然埋词 |
| [post-publish-data-review-workflow.md](playbooks/post-publish-data-review-workflow.md) | 已发布笔记复盘（你发链接 + 后台截图 + 脚本 + 封面，按 24h / 48h / 7d 判断曝光 / 点击 / 完播 / 涨粉卡在哪） |

### 九、知识库 & 跟进 & 产品判断

| 文件 | 能做什么 |
|---|---|
| [content-knowledge-base-workflow.md](playbooks/content-knowledge-base-workflow.md) | 把收藏的笔记 / 链接 / 关键词结果 / 爆款 / 博主沉淀成你自己的知识库（选题库 / 标题库 / 封面库 / 结构库等） |
| [retention-and-follow-up-loop.md](playbooks/retention-and-follow-up-loop.md) | 每次输出结尾给一个具体下一步；密集内容提供 Word / 网页预览 / 知识库打包，不让话掉地上 |
| [product-judgment-and-feedback-loop.md](playbooks/product-judgment-and-feedback-loop.md) | 判断用户真正卡在哪、用人话讲灵造、把反馈转成迭代 |

### 十、运营任务总路由

| 文件 | 能做什么 |
|---|---|
| [xhs-operation-task-tree.md](playbooks/xhs-operation-task-tree.md) | 按具体小红书运营任务（主页诊断 / 对标 / 爆款仿写 / 选题 / 内容 / 封面 / 发布前 / 发布后 / 获客 / 知识库）路由到上面各 playbook |

---

## 能做 / 做不到（免费版边界）

| ✅ 能做（方法论完整） | ❌ 做不到（需付费后端，会诚实告知） |
|---|---|
| 账号诊断、对标筛选与评判、爆款拆解、选题 / 关键词 / 标题 / 封面文案、图文结构、跨平台分发包、发布前检查、数据复盘、知识库沉淀 | 实时结构化抓取小红书 / 抖音数据、保证拿到视频逐字稿、直接生成图片（现改为产 prompt 包，由你自己出图） |

---

## 怎么触发

在支持 Skill 的 Agent 里说「用灵造…」或 `$lingzao`，Agent 会先读 [SKILL.md](SKILL.md)，再按你的任务进入对应 playbook。

示例：

- 「用灵造帮我找 5 个持续更新、有爆款的小红书对标账号」
- 「用灵造完整拆一下这条小红书笔记为什么爆」（需附笔记链接 / 截图 / 文案）
- 「用灵造拿我的账号和 5-15w 粉 AI 博主横向对比」
- 「用灵造给我做一条女性成长图文内容包」
- 「用灵造检查我的标题、封面、前三行和关键词有没有埋进去」
- 「用灵造根据参考图做一张小红书封面」（产出 prompt 包，你自行出图）
