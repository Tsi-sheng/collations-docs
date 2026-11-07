# 命令失败处理与排障

版本：COL-AGENTS-2026-11-07.2。适用于代理执行与故障交接，不改变产品行为。先读 [公共规则](AGENTS.md)和[角色范围](agent-roles.md)，按 [交接模板](handoff-template.md)记录。

## 通用顺序

先记录命令、实际目录、退出码、时间和脱敏错误；再核对仓库/路径、工具、认证/网络、沙箱与真实文件权限。仅采取已授权范围内的最小修正，停止依赖失败结果的步骤，继续独立检查。根因缺证据时写“尚未确定”。不要完整打印环境变量、Token、Cookie 或临时授权下载地址。

确定性失败不原样重试。只有瞬时故障有证据或前置条件已改变时才重试，并记录变化；瞬时诊断最多追加两次。产品自身的重试策略和测试等待期限不因本规则变化。重新连接成功、提权成功或某一步通过都不证明最终功能恢复。

## 常见错误

| 错误 | 先核对 | 范围内处理与停止条件 |
|---|---|---|
| 路径不存在、仓库找错 | `Get-Location`、`Test-Path -LiteralPath`、`.git` 与真实工作树路径 | 显式绝对 workdir；搬迁后检查双向指针，修复须单独授权并备份；不重新初始化或重复迁移 |
| Git dubious ownership | 目录身份、所有者及用户授权 | 已确认目标可用 `git -c safe.directory=<准确绝对路径> -C <同一仓库> status`；不设 `safe.directory=*` 或全局信任 |
| cargo/rustc/cl 等找不到 | `Get-Command`、已知安装位置及当前 shell PATH | 区分未安装/不在 PATH，不擅自安装或改全局环境；独立前端检查继续，原生构建可用已有 CI 并注明来源 |
| GitHub 认证/443/权限 | 插件结果、HTTP 错误类别、仓库权限和实际执行身份 | 优先现有 GitHub 插件；宿主凭据按工具权限申请，不输出凭据，不重复迁移；认证成功不等于仓库可读 |
| 写入拒绝、os error 5、EBUSY | 工具沙箱拒绝还是应用文件系统错误、具体目标、操作、目录和有证据的占用进程 | 不归咎环境或产品；瞬时占用才有限重试，不全局关进程/改权限；目标未知如实记录未知，禁止伪造占用结论 |
| SQLite unable to open database file | 数据库绝对路径、父目录、执行身份、权限和连接 | 分别记录应用与测试进程结果；不删除 Manifest/正文，派生库重建只在已授权入口和范围内执行 |
| 调试端口占用 | `Get-NetTCPConnection` 的端口/PID 与本任务进程归属 | 当前四项 smoke 使用 19341，严格串行；只清理确认由本任务启动的进程，不终止未知程序 |
| Figma INVALID_ARGUMENT | 非敏感文件/节点标识、操作、时间和完整脱敏错误 | 同条件停止请求，有新证据/访问状态变化才验证；认证/会话成功不能证明读文件或截图成功，不擅自新建诊断文件 |
| Git author identity unknown | 仓库身份配置及另有证据的当前授权 | 必要时用命令级已确认身份；旧提交作者仅作线索，不能自动认定身份授权，不编造或修改全局作者 |
| 产物来源不明 | 代码 SHA、CI、归档摘要、exe 摘要 | 核实后才验收；旧 exe 通过不证明新提交；复用 node_modules 不叫干净安装 |

端口/PID 查询结果和文件句柄线索只是证据，不是结束进程的授权。AGENTS 不替代沙箱权限；自动审批拒绝后报告动作、原因和影响，不换工具/写法规避，继续不受影响的工作。

## PowerShell 退出码示例

原生命令失败会设置退出码；不要在读取退出码前运行其他原生命令。使用 `npm.cmd` 可避免误选受执行策略限制的 npm.ps1，但不能绕过工具权限或策略拒绝。

```powershell
npm.cmd run build
$buildExit = $LASTEXITCODE
if ($buildExit -ne 0) {
    [Console]::Error.WriteLine("构建失败，退出码 $buildExit；不执行依赖该产物的步骤")
    exit $buildExit
}
# 只有构建成功才进入依赖步骤。
```

示例使用标准错误输出而非 `Write-Error`，避免 `$ErrorActionPreference='Stop'` 使脚本先抛错并丢失原始退出码。

若输出经过 `Tee-Object`，管道结束后立即保存原生命令的 `$LASTEXITCODE`；同时使用终止错误处理日志写入失败。PowerShell cmdlet 错误用 `-ErrorAction Stop` 或 `$ErrorActionPreference='Stop'` 和 try/catch 检查，不能把上次的 `$LASTEXITCODE` 当作 cmdlet 结果。

## 已知故障边界

既有协调 smoke 两次 os error 5 的目标/占用根因仍未确定；Figma 故障亦未解决。本说明是操作指导，不是上述故障修复记录。具体证据分别见既有回归交接与 [Figma 记录](../ui/2026-11-07-figma-connector-incident.md)。
