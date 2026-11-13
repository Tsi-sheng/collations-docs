# 当前任务板

更新2026-11-13（Asia/Shanghai）。main维护；当前表优先于历史记录。基线见[交接](../../HANDOFF.md)，不启动后台任务。

| ID | 负责人/范围 | 状态/下一关口 |
|---|---|---|
| COL-CLOSE-03 | main文档，review只读；docs75255b2 | 交接收尾提交8b4b3ae，待文档PR审查 |
| COL-TABS-01 | ui/regression/review；85237f5→94a23f2 | 实现及限定回归完成，桌面PR#8待审未合并 |
| COL-HIST-DESIGN-01 | main/review；core4866927、desktop94a23f2；仅文档 | [协议接口方案](../specs/history-v0.4.md)及[ADR0008](../adr/0008-history-store.md)待审，不写产品 |
| COL-HIST-A | main；协议审查后独立core分支 | 未开始：对象、显式提交、完整性与比较 |
| COL-HIST-B | main原生、ui任务视图、regression | 未开始，依赖A：恢复前保全与事务恢复 |
| COL-COMPOSE-C | main核心、ui呈现 | 未开始，依赖固定版本读取；两讲义复用及冻结重建 |
| COL-RELEASE-05 | main/regression | 未开始；检索/图谱缺口、备份和安装 |
| COL-WIN-MANUAL | regression协调真人 | IME/DPI/安装/性能尚未验证 |
| COL-WIN-02 | main单独分配 | os error 5根因未知，有新复现再诊断 |

高级拆分/多窗口只设计；Codex、Capture、AI、同步、移动、Mac Bridge后排。新UI仍Figma优先，旧例外不自动扩展。本批无UI改动，不生成假截图。

## 历史阅读方式

以下“候选”“未开始”为当时快照，不覆盖当前表；准确历史版本与测试保留。

# 历史任务记录（2026-11-07 起）

核实日期：2026-11-07（Asia/Shanghai）。由主控维护；执行窗口提交交接，不同时改此表。开始执行前核对远程与任务授权，本表不启动后台任务。

| ID | 负责人 | 起点与允许范围 | 验收与当前状态 |
|---|---|---|---|
| COL-UI-01 | ui | docs #9；Figma 原文件；交互说明及设计资产 | 轻量写作、面板切换、返回原位置、用户主动设置记忆；画布读取 INVALID_ARGUMENT，设计与截图阻塞 |
| COL-WIN-01 | regression | desktop `678e47e`；tests/**、单独验收记录 | 本机已执行：windows/editor/navigation 通过；reconciliation 两次持久化拒绝访问，未通过；证据待 main 核对归档 |
| COL-UI-02 | ui | COL-UI-01 设计验收后，重新核对 desktop HEAD；src/** 呈现层 | 按设计落地；保留 CM6 实例、撤销/选区/滚动、原始字节、安全保存和 Manifest v3；尚未开始 |
| COL-REVIEW-01 | review | 主控给定 base..head；只读 | 核对 UI 与回归交接、缺陷及遗漏；尚未开始 |

## 已确认的产品方向

默认写作界面轻量，右侧辅助按需打开，维护操作进入菜单。借鉴熟悉的文件/标签操作，不复制未经许可的资产。偏好采用“稳定默认＋手动记忆”，不按使用频率自动改变布局或快捷键。

当前代码已支持主题记忆与编辑会话。单击临时标签、双击固定、导航前进后退、全套偏好持久化及分层设置是候选交互，尚未获整套设计验收，不得写成已实现或全部已确认。

## 并行限制

UI 与回归可以独立准备。回归窗口不得修改 src/** 或 src-tauri/**；发现产品缺陷后上报主控再重新分配。UI 视觉实现等待 Figma 设计与截图；回归可先推进。原生保存、Manifest、接口、依赖/锁文件变更必须由主控单独明确授权，不能在两个窗口中并发修改。

四项打包测试共用 19341 端口，始终串行。测试不得使用私人笔记；没有真人参与时，中文 IME 手工验收保持未验证。

## 四对话加载与后续

- COL-FLOW-01：main 维护公共/角色规则，ui 仅在 docs/development/handoffs/COL-FLOW-01-ui.md 写一份规则加载交接，review 只读审查本轮文档，regression 在既有回归安全结束后确认加载；不再运行 smoke。
- COL-WIN-02：协调持久化 os error 5 的后续诊断，待 main 单独分配，不属于本轮规则配置；不得自行修改原生实现。
- 当前公共规则版本 COL-AGENTS-2026-11-07.2。main 保持单一任务板写入者；不得因规则初始化重复启动 COL-WIN-01。

## COL-FLOW-01 完成记录

四对话已确认加载 COL-AGENTS-2026-11-07.1；ui 小型文档派发、交接和 main 复核已完成，review 在指定提交范围只读审查未发现可复现缺陷。详见 [当次交接](handoffs/COL-FLOW-01-main.md)。应用 cwd 绑定待调整，执行命令使用显式绝对目录。下一项 COL-WIN-02 仍待分配；未重复启动回归或 Figma 请求。

## COL-CMD-01：命令失败规范

当前公共规则 COL-AGENTS-2026-11-07.2，固定提交 46ea6cf。新增 [排障说明](command-troubleshooting.md)，失败记录进入交接模板。只更新文档和读取入口；原生退出码 7、依赖步骤跳过及独立检查继续已做无写入验证。四角色重载已确认，review 指出的退出码 P2 已修正并复核关闭，不重新启动 smoke，不修 os error 5 或 Figma。历史交接中的 .1 保留其当时版本。

COL-CMD-01 完成证据见 [当次记录](handoffs/COL-CMD-01-main.md)。最终固定 SHA 为 46ea6cf；未知故障仍待单独分配。

## COL-UI-DESIGN-02：完整工作区设计续作

main协调，ui为唯一画布写入者，review只读审查交互及数据边界。新增 [交互规范](../ui/workspace-customization.md)及[状态清单](../ui/workspace-customization-states.md)，目标为三批画板、两条原型。当前仅文档草案；Figma访问及本次截图未验收。不修改desktop PR #5，不运行smoke，不开始独立窗口实现。原有COL-UI-01轻量布局需求由该规范继续覆盖。

COL-UI-DESIGN-02规范修订已获review定向复核：1P2+2P3关闭，24逻辑状态/2原型路径保留。用户确认权限或连接已调整后，ui的create_session遇连接传输失败，根读取未执行；Figma画板和截图仍未完成。详细证据见 [交付记录](../ui/2026-11-07-workspace-customization-delivery.md)。

## COL-UI-IMPLEMENT-01：单窗口前端接入

用户批准本批按已审查规范先实现，以真实程序截图验收，Figma新图保持未完成；例外仅本批。范围见 [实施约定](../specs/single-window-ui-integration.md)。ui在干净678e47e基础独占src和获准tests，main写文档，regression只读准备待固定提交，review后续只读审查。core本轮类型检查与12测试已通过；不改native持久化，不开始COL-WIN-02。桌面 PR #7 的最终构建 85237f5 在真实 Windows 上七项 smoke 全部通过，已保存四种视口实机截图。见 [Windows 验收与定向复验](../ui/2026-11-12-85237f5-acceptance.md)。Figma、真人 IME、实际系统缩放、性能与安装验收仍待执行。
