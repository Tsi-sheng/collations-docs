# Collations 当前接续交接

核实日期：2026-11-13（Asia/Shanghai）。本轮只更新文档、审查下一阶段协议，不重跑产品测试。开工重新核对远程 HEAD、PR、依赖与 artifact；不自动合并。

## 当前固定基线

| 仓库 | SHA 与入口 | 状态 |
|---|---|---|
| core | 4866927187e5bcaca4c76238ac12e41a37a922f9，[PR #5](https://github.com/Tsi-sheng/collations-core/pull/5)，feat/knowledge-workspace-v0.3 | Open / Draft；身份、协调、预览和链接基础；历史/汇编未实现 |
| desktop | 94a23f275fd80a142eadc85dceddf8bdbe32557f，[PR #8](https://github.com/Tsi-sheng/collations-desktop/pull/8)，codex/restore-tab-shortcuts | Open / Ready for review；固定依赖上述 core；单窗口、设置、导航和标签快捷键 |
| docs | 本文档分支基于 75255b2445b73bed915cc418d3fd75906e763e3f，[PR #12](https://github.com/Tsi-sheng/collations-docs/pull/12)，docs/single-window-ui-integration | Open / Draft；本批文档 PR 继续堆叠 |

web/mobile/api 保留浏览器、移动平板、远程服务职责，不参与日常构建。本日未重审三仓内容；README-only 仅为[11月6日历史快照](HANDOFF-2026-11-06.md)记录，不是本日结论。

## PR 依赖

本日查询的既有 core 6、desktop 8、docs 12 个 PR 均 Open、未合并。准确 HEAD、base、Draft 状态见[机器快照](docs/development/evidence/2026-11-13-pr-baselines.json)。本批新增文档 PR 不计入这次历史查询总数。

- core：#1 → #2 → #3 → #4 → #5；README #6 基于 #5。
- desktop：#1 → #2 → #3；#5 基线 feat/knowledge-workspace-v0.3 位于 #3 的 7aa42af；#5 → #7 → #8。README #4 基于该 v0.2 基线，规则 #6 基于 #5，均非 #8 的后继功能。
- docs：#1 → #2 → #3 → #4 → #6 → #7 → #8 → #9 → #10 → #11 → #12；Capture #5 从 #1 独立分出。

箭头表示前置，不表示合并授权。main 不等于功能基线，不重做迁移或重置历史。

## 程序与证据

[收尾验收记录](docs/development/handoffs/2026-11-13-v03-closeout.md)区分版本、结果及限制。

- [CI 43372598325](https://github.com/Tsi-sheng/collations-desktop/actions/runs/43372598325) / [x64 artifact 14012728539](https://github.com/Tsi-sheng/collations-desktop/actions/runs/43372598325/artifacts/14012728539)：Release exe，不是安装包。
- exe SHA-256：6c880ff5856b56c033d724675fcee89b0d14ffd1eab5a5ec461b18d3083455ba。
- ZIP SHA-256：8cb278c2b5bedccd269c0dc7423d653219a4979de4d2ef18bf087d793b48596a。
- 本日 GitHub 返回未过期，到期 2027-02-11T05:31:41Z（北京时间13:31:41）；非永久发布，过期后按固定提交重建。
- [85237f5 历史回归及截图](docs/ui/2026-11-12-85237f5-acceptance.md)不能代替新版本结果。

## 接续与环境

原生 Markdown、BOM/换行/未知元数据、Document ID、Manifest v3、安全保存、草稿及离线能力保留。SQLite仅派生；监听事件不直接证明身份。大纲读取缓冲区，关系读取最近保存版本。

下一步：收尾 → v0.4-A显式历史 → v0.4-B安全恢复 → v0.4-C Markdown讲义 → v0.5备份发布。见[任务板](docs/development/task-board.md)。本批只协议接口审查，不启动核心实现、汇编UI、多窗口或插件。版本、汇编、文献、采集使用独立任务视图。架构中的历史/汇编是目标，不是已实现功能。

core 根目录：npm ci、npm run check、npm test；协议样例：node --test tests/protocol.test.mjs。desktop 根目录：npm ci、node --experimental-strip-types --test tests/*.test.mjs、npm run build。Windows CI 执行 npm run tauri -- build --no-bundle 和 cargo test --locked --release --manifest-path src-tauri/Cargo.toml，产物 src-tauri/target/release/collations-desktop.exe。

打包脚本需交互式Windows、匹配exe及独立合成样例；先读对应 tests/packaged-app.mjs。端口19341串行使用，记录脚本SHA和exe来源。云端干净检出/安装/私有依赖访问仍需独立验收，CI成功不等于云端已准备。

真人IME、物理键盘/DPI、安装、性能、Figma新图尚未验证。历史os error 5根因未知，未复现不等于修复。Capture独立规划；高级布局先评审单会话/单保存通道；Codex只读接入、Mac Bridge、同步及移动端后排。

## 历史目录保护

旧桌面已归档可达Git refs、补丁及33源文件，独立恢复、SHA-256核对通过；未删除目录或缓存。旧主仓库元数据仍供UI/regression关联工作树使用，不得整目录删除。归档不含忽略缓存、凭据或不可达对象，不是整机备份。本机定位留在协调记录，云端接续不依赖这些路径，不复制Git元数据或重复迁移。

## 下一任务协议入口

[HistoryStore ADR（待审）](docs/adr/0008-history-store.md) · [v0.4协议与接口审查方案](docs/specs/history-v0.4.md)。先审查A的存储与B的恢复边界，再启动A1实现。
