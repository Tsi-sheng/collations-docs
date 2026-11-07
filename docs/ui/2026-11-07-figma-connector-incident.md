# Figma 连接器故障报告

整理时间 2026-11-07 15:05 +08:00。状态为待诊断，用户选择保留当前文件并整理故障材料。报告中的复现来自此前实际工具返回，本次整理没有再次调用画布接口。

## 问题与影响

Collations 既有 Design 文件的根页面读取、节点元数据读取和截图调用返回同一错误。账号查询及文件会话创建成功。无法检查当前画布，因此轻量默认写作画板和本次截图均未交付。

[目标文件与当前页面](https://www.figma.com/design/VzC8WeV0K7pVy2XVKiqqtV?node-id=0-1)

| 项目 | 已知值 |
|---|---|
| 文件类型 | Figma Design |
| 文件键 | VzC8WeV0K7pVy2XVKiqqtV |
| 用户提供的当前页面 | 0:1 |
| 既有浅色画板 | 2:2 |
| 其他已创建画板 | 深色 2:129、默认窗口 2:256、窄窗 2:386 |
| 客户端 | Codex Windows 桌面会话 |
| 技能包 | figma 17.0.0 |
| 认证查询 | whoami 成功，handle 为 Viscum |
| 团队与席位 | Starter，View seat，返回 role 为 admin |
| 重新连接 | 用户明确确认已断开并重新连接 |
| 会话创建 | 重新连接后 create_session 成功返回 sessionId |

团队角色、席位字段与实际文件编辑权限的对应关系尚未核实。报告不包含邮件、令牌、Cookie 或授权下载地址。

## 最小复现

操作前使用目标文件键创建会话，保存实际返回的 sessionId。然后使用相同值发送一次只读调用。

工具为 `use_figma`，参数如下。示例中的 SESSION_ID 应替换为本次创建会话的真实返回值。

```json
{
  "fileKey": "VzC8WeV0K7pVy2XVKiqqtV",
  "sessionId": "SESSION_ID",
  "skillNames": "figma-use,figma-generate-design",
  "description": "Read root pages once after the confirmed reconnect",
  "code": "return {pages:figma.root.children.map(p=>({id:p.id,name:p.name}))};"
}
```

预期返回当前文件的页面 ID 和名称。已观察到的完整错误载荷如下。

```json
{"error_code":"INVALID_ARGUMENT"}
```

工具未提供失败字段、堆栈、服务端请求 ID 或更具体的原因。根页面枚举不依赖旧画板节点 ID。

## 已执行步骤

| 检查 | 结果 |
|---|---|
| 使用 whoami 查询账号 | 成功返回账号、计划和席位 |
| 创建原文件会话 | 成功返回 sessionId |
| 读取浅色画板 2:2 元数据 | INVALID_ARGUMENT |
| 截取浅色画板 2:2 | INVALID_ARGUMENT |
| 使用用户重新提供的链接读取页面 0:1 | INVALID_ARGUMENT |
| 使用只读 use_figma 枚举根页面 | INVALID_ARGUMENT |
| 用户确认重新连接后创建新会话并执行一次根页面枚举 | 会话成功，根读取仍为 INVALID_ARGUMENT |

2026-11-07 14:55:34 +08:00 对画板 2:2 的 get_metadata 调用失败。2026-11-07 14:57:56 +08:00 已记录对用户提供的当前页面和根枚举的复核。重新连接后验证的准确调用时间未单独保存，可由原会话工具日志定位。

此前该文件曾成功创建画板和截图，随后也曾收到明确的 Starter 工具调用额度提示。当前只返回 INVALID_ARGUMENT，不能根据历史额度提示确定当前根因。

## 尚未确定的原因

以下内容需要服务端日志或新的用户证据验证。

- fileKey、sessionId 或节点参数是否在连接器到 Figma 服务之间发生校验或转换问题。
- 账号席位、文件归属和文件级权限是否满足当前调用要求。
- 当前是否存在额度或服务限制，以及该限制是否被映射为此通用错误。
- 问题是否仅影响该文件。用户选择保留现状，尚未建立独立诊断文件进行对照。
- 浏览器是否有具体错误或权限提示。用户提供了文件链接，尚无浏览器提示截图。

## 给支持人员的排查请求

请核对上述最小读取请求，确认 INVALID_ARGUMENT 对应的失败字段或校验阶段，并检查文件访问、会话关联及限制状态。若需要补充材料，请明确所需客户端版本、请求追踪标识或脱敏日志范围。

## 接续条件与验收

获得可解释的诊断结果或连接／文件访问状态发生变化后，再做一次根读取验证。读取成功后，按 [轻量写作设计说明](lean-writing.md) 在原文件修订页面，保留此前画板。新截图应满足 [UI 工程约定](AGENTS.md) 并持久保存。

当前完成的是故障材料整理。Figma 修订、画板视觉检查及本次截图验收保持未完成。

[完整交付记录](2026-11-07-delivery.md)
