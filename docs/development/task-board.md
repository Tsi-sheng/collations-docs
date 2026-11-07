# 当前任务板

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
