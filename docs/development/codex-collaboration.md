# Codex 多对话接续开发

核实日期：2026-11-07（Asia/Shanghai）。本轮调整开发协作，不改变六仓结构、产品协议或已有 PR 的合并状态。参考用户提供的「Codex套餐选择分析」对话；其中目录与模型建议按实际仓库校正，不照搬示例。

## 先读什么

依次读取根 AGENTS.md、[任务板](task-board.md)、[v0.3 需求](../specs/windows-v0.3.md)及相关 ADR。UI 任务另读 [UI 约定](../ui/AGENTS.md)、[轻量写作说明](../ui/lean-writing.md)与 [Figma 故障记录](../ui/2026-11-07-figma-connector-incident.md)。旧 HANDOFF.md 是历史快照，当前实现起点以下表为准；开工仍须重新核对远程。

| 仓库 | 核实基线 | 当前职责 |
|---|---|---|
| core | `4866927187e5bcaca4c76238ac12e41a37a922f9` | 身份、Manifest v3、协调、解析与链接基础；本轮不重写 |
| desktop | [PR #5](https://github.com/Tsi-sheng/collations-desktop/pull/5)，`678e47e29b205893be67ed26b7dbd7a3ebea3038` | 已接 core 上述 SHA；继续 UI 与最终打包回归，PR Open/Draft |
| docs | [PR #9](https://github.com/Tsi-sheng/collations-docs/pull/9)，`ed7b4cf9bbd4d841c1f5e968f4045c71c9cb0bbe` | UI 规则与故障材料，PR Open/Draft；本协作文档堆叠于其上 |

web、mobile、api 仍按既定职责保留，不参与本轮日常构建。不得将外层快照仓库作为六仓的替代历史，也不得重新初始化真实仓库。

## 对话与模型

保留一个主控对话，新增两个执行对话，审查对话按需建立。保留会话不等于持续运行；同一时间最多两个写入者，每个任务必须指定文件范围。同一 Figma 文件只由 UI 对话写入。

| 对话名字 | 推荐模型与推理 | 工作方式 |
|---|---|---|
| Collations｜主控与集成 | GPT-6.1 Sol / Medium，复杂接口或风险评估临时 High | 核对基线、任务分配、文档、串行集成；执行者活跃时不改其文件 |
| Collations｜UI 设计与交互 | GPT-6.1 Terra / Medium | Figma、截图及获准后的呈现层实现；复杂编辑会话问题交主控判断 |
| Collations｜Windows 回归与可靠性 | GPT-6.1 Sol / Medium | 隔离样例与回归、保存和恢复验证；清晰机械的测试整理可临时用 Luna |
| Collations｜独立审查（按需） | GPT-6.1 Sol / High；难题再用 GPT-6 Astra | 只读审查明确提交范围，报告缺陷，不与实现者同时修复 |

模型是人工选择建议，不是仓库可强制的设置。账号实际模型列表与额度以客户端为准。暂不安装或配置 DeepSeek，不承诺其与当前 Codex 插件、认证及工具协议兼容；额外 API 预算为零时不主动引入付费调用。

官方依据：[模型与推理设置](https://learn.chatgpt.com/docs/models)、[Worktree 隔离](https://learn.chatgpt.com/docs/environments/git-worktrees)。更高推理通常消耗更多时间与 token；仅遇到任务复杂度需要时提高。

## 任务交接和集成

任务以可验收结果划分，不永久绑定“前端/后端”。主控维护任务板，执行者提交[交接记录](handoff-template.md)，不依赖其他对话自动掌握历史。每个工作树独立分支；开始前核对 HEAD、dirty 状态、core 固定依赖及允许修改路径。隔离工作树不会自动同步其他工作树的未提交修改。

执行者只提交自己的差异，禁止直接修改 main、自动合并、强推、清理别人的工作树或将凭据写入仓库。主控核对 diff 和测试后串行集成到功能分支，保持现有堆叠 PR 依赖，最终仍等待用户审查。

测试使用不同临时目录、SQLite 和 WebView 用户数据目录。当前 `tests/packaged-app.mjs` 固定调试端口 19341，四项打包回归必须串行；不要为了并行化在本轮顺手改测试协议。

## 给新增对话的首条消息

**UI 对话：**

> 你负责 Collations 轻量写作 UI。先读取 collations-docs 本分支的 AGENTS.md、docs/development/task-board.md、docs/ui/AGENTS.md、lean-writing.md 和连接器故障记录，核对 desktop PR #5 的最新提交。目标是类似 Obsidian 的熟悉操作、清晰切换和“稳定默认＋手动记忆”。先设计后实现；Figma 仍阻塞时只做可独立验证的诊断和交互说明，不重复失败请求、不新建文件、不伪造截图。保留每标签编辑会话、安全保存、身份与原始字节。仅在任务板授权范围内修改，交付实际截图、节点、提交与测试，未验证项单列。

**Windows 回归对话：**

> 你负责 desktop PR #5 精确提交的 Windows 验收。读取 docs/development/task-board.md、相关 ADR 和 desktop README，先报告 HEAD、工具链和实际产物。四项 smoke 串行执行，各用隔离样例，核对 exe SHA-256、Markdown 字节和 SQLite 重建。保存日志，区分已有问题、环境限制和新增回归。默认只修改 tests/** 和验收记录；产品缺陷回报主控，由主控分配修复。不得改变 Manifest、保存契约、core 依赖或 UI 文件，不声称真人 IME 已验证。

**审查对话：**

> 只读审查主控指定的 base..head；先读取工程约定和任务交接，输出可复现缺陷、位置、影响、缺失测试及优先级。重点检查身份、字节保真、保存竞争、恢复和布局切换状态。不要修改文件、合并 PR 或重复全仓设计。

## 本轮已做与未做

已核对远程 PR #5/#9 并下载对应 Git 对象；保留原 desktop 脏工作树。搬迁造成 docs-v011 双向 Git 路径失效，备份两份指针后通过 `git worktree repair` 修复；本分支从 docs #9 的确切提交继续。

已为真实 desktop 仓库创建两个 Codex 托管隔离工作树，起点均为 `678e47e`。外层 main/editor/storage-tests/review 是本地快照的工作树，不包含被忽略的真实 desktop/core 仓库，不得直接用来开发最新产品。机器路径与指针备份留在本机启动说明，不作为云端依赖。

本轮未创建新对话、未替用户切换模型、未配置 API、未变更产品代码或重跑 Windows 发布验收。新工作树尚未安装依赖及完成独立构建验收。Figma 故障仍未解除，本轮为开发组织调整，没有新的 UI 截图。

## 本轮验证

在搬迁后的原 core 目录执行 `npm run check` 和 `npm test`，类型检查通过，12/12 测试通过（Node 提示 SQLite 为实验特性）。外层快照根目录执行 `node --test tests/protocol.test.mjs`，2/2 通过；这是迁移样例校验，不替代 desktop 打包测试。新增入口及文档相对链接全部解析，`git diff --check` 通过。原 desktop 的 src、src-tauri/src、tests 共 20 个文件与本轮操作前 SHA-256 一致。
