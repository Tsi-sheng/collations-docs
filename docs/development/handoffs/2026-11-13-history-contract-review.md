# COL-CLOSE-03 / COL-HIST-DESIGN-01 交接

日期2026-11-13，Asia/Shanghai。main负责文档，review只读。当前批次已完成交接收尾与协议/接口审查准备，不包含历史核心、原生恢复、汇编或新UI实现。

## 分支与证据

- [收尾PR #13](https://github.com/Tsi-sheng/collations-docs/pull/13)：基于docs PR #12的75255b2445b73bed915cc418d3fd75906e763e3f；收尾提交8b4b3aec5befae69a01baffcccb317b5c9527323，空行修正c7a2c5c。
- [协议PR #14](https://github.com/Tsi-sheng/collations-docs/pull/14)：基于收尾分支，保留普通合并历史；契约最终复审SHA 08b127f211aef263071acd2fcecbc09c69a008ac。
- 本记录只补交接与状态，不改变已复审契约。PR待用户审查，不自动合并，ADR仍为待审、未发布格式。
- [当前交接](../../../HANDOFF.md)、[ADR0008](../../adr/0008-history-store.md)、[接口与验收方案](../../specs/history-v0.4.md)、[路线图](../../ROADMAP.md)。

## 独立审查结果

收尾状态与原始验收记录、三仓PR快照一致。协议审查四项P2均在文档层面关闭：

1. 恢复历史原始字节时不能假定移动后的相对引用仍有效；首轮路径差异及引用身份变化先阻止恢复并要求协调。
2. 发布前增加最终来源/清单/HEAD校验，明确采集集合不是OS原子时点快照。
3. 父清单摘要链保护旧Revision清单，防止合法JSON替换成另一合法对象而静默通过。
4. HEAD缺失且有任何历史痕迹时保守只读，不能凭首次准备记录自动建立新根；需显式恢复核验。

上述是方案审查，不是实现正确性或故障恢复测试通过。实际样例、校验器、接口代码和故障注入属于下一实施任务。

## 本次验证及失败处理

- 检查Git工作树、固定提交、远程分支和三仓全部既有PR；GitHub读取成功。core/desktop保持4866927/94a23f2且干净。
- 使用现有mdast-util-from-markdown解析全部变更Markdown，检查一级标题、代码围栏及本地链接；无新依赖。JSON快照解析并核对6/8/12个既有PR。
- 完整文档差异执行git diff --check（CRLF感知），只包含Markdown和状态JSON，无产品代码、锁文件、凭据、缓存或本机角色配置。
- 未重跑产品测试、构建或Windows smoke。旧程序验收按版本引用，不制造本次测试成绩。未清理旧目录/归档。
- 首次生成文档时Python subprocess以GBK解码gh的UTF-8输出失败，exit1；当时仅新增历史副本，后续写入停止。改为encoding=utf-8后生成成功。中间工具编排曾因字符串转义返回SyntaxError、无shell执行；改用正确引用后继续。均非产品错误或权限变更，无凭据修改。

## 下一任务及门禁

当前只启动了收尾与协议接口审查。等待PR审查后，main分配COL-HIST-A的A1：版本化样例、格式校验、纯函数与内存适配；随后A2真实文件/发布事务，A3读取比较。B安全恢复与UI、C讲义汇编均未开始。没有新增公共API、没有迁移Manifest、没有安装工具链。

ui在历史接口审查后再做独立任务视图，按Figma规则执行；regression收到固定实现SHA后才跑测试；review继续独立审查身份/发布/恢复。真人IME、DPI、安装、性能、云端准备及历史os error 5保留独立任务。高级布局、插件、Capture/AI、同步及Mac Bridge后排，不据本次文档自动开工。
