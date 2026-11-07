# 执行窗口交接模板

每个任务建立独立交接文件或在 PR 正文填写，不共写同一结果文件。

```yaml
task_id: COL-XXX
owner_chat: main/ui/regression/review
rules_version: COL-AGENTS-2026-11-07.2
actual_workdir: 实际执行目录
app_binding: 已对应/待调整
repository: Tsi-sheng/collations-desktop
base_commit: 完整 SHA
head_commit: 完整 SHA（未提交则明确注明）
branch: 功能分支
scope: [允许修改的路径]
changed_files: [实际修改的路径]
dependencies: [依赖任务或固定 core SHA]
tests:
  - command: 实际命令
    environment: 操作系统和工具链
    result: 通过/失败/未执行，数量与日志位置
failures:
  - command: 失败命令（凭据和签名地址脱敏）
    actual_workdir: 实际执行目录
    exit_code: 原生命令退出码；工具失败则标明无退出码
    error: 脱敏错误原文
    evidence_checked: 已检查的路径、工具、权限或占用证据
    retries_and_changes: 次数、各次条件变化；未重试则注明
    impact: 被阻塞的依赖步骤与可继续部分
    root_cause: 尚未确定，或有证据的结论
    next_action: 范围内下一步或回 main 分配
known_issues: []
not_verified: []
next_action: 下一步或需要主控决定的内容
```

UI 任务追加本次 Figma 节点、截图、视觉检查与资产 SHA-256；失败记录具体错误，不用历史截图冒充。Windows 交付追加 exe SHA-256、来源提交、CI/产物链接及归档摘要，分别记录真实系统缩放与模拟视口、真人 IME 与自动文本输入。
