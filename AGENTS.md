# Collations Docs 入口

规则版本：COL-AGENTS-2026-11-07.2。这是重建后的现行入口。

先读取 [固定版本公共规则](https://github.com/Tsi-sheng/collations-docs/blob/46ea6cfdfbfa88f99d9bdca7593781940849a89e/docs/development/AGENTS.md)与[四对话角色](https://github.com/Tsi-sheng/collations-docs/blob/46ea6cfdfbfa88f99d9bdca7593781940849a89e/docs/development/agent-roles.md)；本地对应文件为 [公共规则](docs/development/AGENTS.md)和[角色规则](docs/development/agent-roles.md)。核对版本一致再执行。[任务板](docs/development/task-board.md)由 main 维护。

实现状态以当次 SHA 与证据为准。文档协议导航见 README 和 PROJECT.md。不直接改远程 main、不合并 PR、不强推，保留用户改动。UI 工作另读 [UI 约定](docs/ui/AGENTS.md)，保持先 Figma、实际新截图及失败据实报告。

命令失败处理遵守 [固定排障说明](https://github.com/Tsi-sheng/collations-docs/blob/46ea6cfdfbfa88f99d9bdca7593781940849a89e/docs/development/command-troubleshooting.md)：保留退出码与证据，依赖失败停止、独立工作继续；无条件重复失败和绕过权限均不允许。
