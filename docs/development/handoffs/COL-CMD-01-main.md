# COL-CMD-01：命令失败规则验证

日期：2026-11-07，Asia/Shanghai。公共规则 COL-AGENTS-2026-11-07.2，最终固定快照 46ea6cfdfbfa88f99d9bdca7593781940849a89e。

## 已实现

公共 AGENTS 增加命令失败处理，排障说明覆盖路径/仓库、Git 所有权、工具链、认证/网络、写入/文件占用、SQLite、端口、Figma、作者身份和产物来源；交接模板新增退出码、证据、重试条件、影响、根因和下一步。入口引用固定提交，本机覆盖仍排除在产品 PR 外。没有修改产品代码、依赖、权限配置、构建脚本或全局模型。

## 无写入验证

- `node -e "process.exit(7)"` 的输出经过 `Tee-Object -Variable` 后立即捕获退出码，结果仍为 7；依赖分支未执行，独立 Test-Path 成功。
- 用 try/catch 捕获故意不存在的 Get-Command，确认 cmdlet 错误与原生退出码分别处理。没有安装工具或修改 PATH。
- review 独立复现 P2：旧示例使用 Write-Error，在 ErrorActionPreference=Stop 下使后续 exit 未执行，调用者收到 1。main 将其改为 Console.Error.WriteLine。
- 修正后实际运行独立 PowerShell 子进程，设置 ErrorActionPreference=Stop；观察调用者收到 7，依赖步骤标记未出现。此检查没有文件写入、数据库操作或权限修改。
- Markdown 链接/围栏、角色映射和本机排除规则验证通过；原 desktop 20 个源/测试文件 SHA-256 与既有基线一致。历史 .1 交接保持原样。

## 审查与重载

review 对 docs 3213360..85d0450 和 desktop 2a43b63..99c8f80 只读检查，报告上述 P2，其余指定检查未发现可复现缺陷。修正已提交为 46ea6cf，并单独请 review 复核修正、请 ui/regression 确认最终固定引用。最终加载与复核结果由 main 在本记录追加，不将初次审查误记为无问题通过。

## 未解决

原生持久化 os error 5、Figma 访问、真人 IME、真实 Windows 缩放及安装环境仍为独立待办。本轮不运行完整构建或 smoke，不认为 AGENTS 能强制执行权限，也不声称应用 cwd 已重新绑定。

## 最终确认

review 只读复核 85d0450..46ea6cf 后确认原 P2 已解决；ui、regression、review 均确认 .2 和修正后固定 SHA。入口及版本文字的后续同步由 main 检查，不扩张此前代码审查范围。最终链接/围栏/绑定检查通过，20 个原始源/测试文件摘要保持一致；未重新执行产品回归。
