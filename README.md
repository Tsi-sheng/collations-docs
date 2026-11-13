# collations-docs

> 当前入口（2026-11-13）：[HANDOFF](HANDOFF.md) · [任务板](docs/development/task-board.md) · [94a23f2证据](docs/development/handoffs/2026-11-13-v03-closeout.md)。旧版本引用保留为历史来源。

## 定位与状态

Collations（秩笺）项目与平台文档仓库，维护架构、协议、ADR、路线图，并逐步补充使用指南、教程及开发文档。**活跃维护，保留现有名称。**

历史README基线（2026-11-06）：本说明原对应 [`ab913b0`](https://github.com/Tsi-sheng/collations-docs/tree/ab913b063437d9ba31ba19f9029167ecbd16626d)（`feat/knowledge-workspace-v0.3`），[设计 PR #4](https://github.com/Tsi-sheng/collations-docs/pull/4) 尚待审查。规范描述目标与约束，不代表全部功能已经实现。

## 职责边界

| 仓库 | 当前状态 | 职责与处理建议 |
|---|---|---|
| [collations-core](https://github.com/Tsi-sheng/collations-core/tree/4866927187e5bcaca4c76238ac12e41a37a922f9) | 活跃开发 | 解析、身份、Manifest、协调和链接基础；保留 |
| [collations-desktop](https://github.com/Tsi-sheng/collations-desktop/tree/7aa42af1a34991f4d47bb03cc1f1194bd254545d) | 活跃开发 | Tauri 桌面应用、文件/监听/SQLite、编辑器；保留 |
| [collations-docs](https://github.com/Tsi-sheng/collations-docs/tree/ab913b063437d9ba31ba19f9029167ecbd16626d) | 活跃维护 | 项目总览、规范、ADR 和验收记录；保留 |
| [collations-web](https://github.com/Tsi-sheng/collations-web/tree/205046d0521650333f8587bbce6b56f0b68f5372) | 规划中 | 浏览器客户端预留；保留 |
| [collations-mobile](https://github.com/Tsi-sheng/collations-mobile/tree/66aa02a34bfd6146ab9923823541584cea66ddf9) | 规划中 | 移动与平板客户端，包括 HarmonyOS 路线；保留 |
| [collations-api](https://github.com/Tsi-sheng/collations-api/tree/cd13eccde781cb5e982be58ce0a3b2301c5880a5) | 规划中 | 后续同步、分享及远程服务；保留 |

后三仓在核实基线仅有 README，没有可运行产品。它们承担不同的未来职责，没有需要改名或删除的重复实现。本轮也不执行归档。桌面端属于 desktop，不属于 web。

## 现有能力与规范导航

- [项目、架构与协议目录](PROJECT.md)
- [架构及平台规范](docs/ARCHITECTURE.md)
- [数据、链接、版本和汇编协议](docs/PROTOCOL.md)
- [路线图](docs/ROADMAP.md)
- [身份决策](docs/adr/0001-identity.md)与 [Windows 索引恢复](docs/adr/0002-windows-index.md)
- [Manifest v3 协调与恢复](docs/adr/0004-file-reconciliation.md)及 [v3 样例](docs/examples/manifest-v3.json)
- [编辑会话与安全保存](docs/adr/0005-source-editor.md)
- [v0.3 工作区 UI 与链接索引设计](docs/adr/0006-workspace-ui.md)

原始 [core 协议样例版本](https://github.com/Tsi-sheng/collations-core/tree/2ac06c13549469b57696ec99e33056ad051ff9f9) 继续作为协议迁移基线。v3 身份协调和 v0.2 编辑器已有实现；core v0.3 链接基础已提交，desktop 单窗口UI、关系索引及标签切换已在94a23f2分支实现，限定验收见当前交接，尚未合并。版本仓库和完整汇编器仍属于后续里程碑。

[Capture 设计 PR #5](https://github.com/Tsi-sheng/collations-docs/pull/5) 位于独立分支；其中回归规范是未来实现的验收设计，不是已执行的 Capture 测试。旧协议和路线图中的目标以对应阶段实现及测试记录核实，持久格式以相关 ADR 的版本说明为准。

## 开发与验证

本仓没有应用构建或自动化测试运行器。修改文档时检查 Markdown 结构、相对链接、跨仓库版本引用和差异范围；涉及持久数据协议时必须同时提供 ADR、格式版本、迁移样例与恢复测试。

数学协议测试在 core 的相应版本根目录运行：

```sh
node --test tests/protocol.test.mjs
```

当前 core 检查命令见 [core 文档](https://github.com/Tsi-sheng/collations-core/blob/4866927187e5bcaca4c76238ac12e41a37a922f9/README.md)及其 package.json；Windows 构建和测试入口见 [desktop 文档](https://github.com/Tsi-sheng/collations-desktop/blob/7aa42af1a34991f4d47bb03cc1f1194bd254545d/README.md)。测试记录必须包含提交、执行环境、命令与结果；区分历史结果、当次执行、尚未验证及已知问题。

## 云端工作方式

工作流为：Codex 云端准备依赖与修改代码 → 功能分支与 PR → CI 验证 → 人工审查。开发环境的联网安装不改变应用离线工作的要求。不得直接修改 main、自动合并 PR、强制推送或将凭据写入仓库。

日常环境优先纳入 core、desktop、docs，后三仓按需加入。开始任务先核对远程分支和 PR：当前 main 尚未包含全部功能分支，不得假设默认检出就是最新实现。跨仓库联调记录双方 SHA，交付依赖使用固定提交及锁文件。

Codex 云端承担代码与通用验证，desktop 的 Windows CI 承担原生构建；真实 Windows 文件行为、交互和输入法按对应验收条件执行。开发安装依赖可联网，产品离线工作原则不变。现有 Windows 工作流输出可执行文件，安装包、自动发布和更多 CI 验收属于待办。

## 关联文档和后续工作

以 [路线图](docs/ROADMAP.md) 和上述 ADR 组织里程碑。下一步审查版本历史协议与接口，独立跟进云端环境、真人验收和发布准备；不通过改名、删仓或重写历史来代替职责澄清。

## 云端接续开发

- [多对话协作、模型建议与启动说明](docs/development/codex-collaboration.md)及[当前任务板](docs/development/task-board.md)（当前表优先于历史记录）
- [总交接：基线、PR 依赖、验证与待办](HANDOFF.md)
- [Windows v0.3 最终需求与视觉参考](docs/specs/windows-v0.3.md)


## 工作区 UI 设计续作

[轻量写作与完整定制规范](docs/ui/workspace-customization.md) · [画板、状态与原型清单](docs/ui/workspace-customization-states.md)。这是独立设计扩展，画板与新截图仍待 Figma 访问恢复，不表示多窗口或完整定制已经实现。

## 单窗口前端实施

本批用户批准 [按规范先实施、以真实程序截图验收](docs/specs/single-window-ui-integration.md)；仅本批例外，Figma新图未完成。[UI偏好ADR](docs/adr/0007-ui-preferences.md)记录独立v1本机格式与损坏恢复，不改变Manifest。实现和测试见 [Windows 验收、截图与定向复验](docs/ui/2026-11-12-85237f5-acceptance.md)。
