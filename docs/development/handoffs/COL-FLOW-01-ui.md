# COL-FLOW-01：UI 规则加载交接

- 角色：ui；任务为只读基线核对和本交接，不进行 UI 设计或实现。
- 实际应用 cwd：`<COORDINATION_ROOT>`；目标工作树：`<UI_WORKTREE>`。应用绑定待调整；命令使用绝对仓库路径。
- 仓库：`Tsi-sheng/collations-desktop`；分支：`feat/lean-writing-interactions`；基线与 HEAD：`678e47e29b205893be67ed26b7dbd7a3ebea3038`。只读状态检查未显示工作树改动。
- 规则版本：`COL-AGENTS-2026-11-07.1`；固定公共规则提交：`8d45db4cdc17046acf8e10828dcf778064530e94`。本地[公共规则](../AGENTS.md)和[角色规则](../agent-roles.md)与该提交对应文件无差异；已读取[任务板](../task-board.md)、[UI 约定](../../ui/AGENTS.md)及本机 UI override。UI 工作树根目录无 `AGENTS.md`。
- 本任务允许范围：只新建本交接文件，其他文件、产品代码和任务板不修改；不调用 Figma、不新建设计文件、不运行完整构建、不切换模型、不提交或推送。
- 产品方向为轻量写作、按需辅助面板与稳定默认加手动记忆。临时／固定标签、导航历史、完整偏好持久化和分层设置仍是候选交互，尚待整套设计验收。
- 已知阻塞：任务板记录现有 Figma 画布读取 `INVALID_ARGUMENT`，设计及本次截图验收阻塞；本任务未调用 Figma，未重新验证该错误。
- 验证：在 Windows 沙箱以 `git -c safe.directory=<UI_WORKTREE> -C <UI_WORKTREE> status --short --branch` 和 `rev-parse HEAD` 只读核对。初次不带临时 `safe.directory` 的查询因所有权检查失败；未修改全局 Git 配置。以 `git diff 8d45db4cdc17046acf8e10828dcf778064530e94 -- docs/development/AGENTS.md docs/development/agent-roles.md` 核对固定规则。
- 未执行 UI 设计、视觉检查、截图、产品实现或构建；不声称其通过。后续由 main 核对本交接及链接并决定提交。

本记录由 main 核对后将机器绝对路径归一化；实际路径保存在本机绑定表，不作为云端依赖。
