# 单窗口工作区接入范围

日期：2026-11-07，Asia/Shanghai。任务 COL-UI-IMPLEMENT-01。用户在本轮明确批准先按已审查规范和既有参考实现，以真实程序截图验收；Figma新画板与新截图仍未完成。本例外仅适用于该批次，不修改长期Figma优先规则。

## 基线与范围

Desktop PR #5头为678e47e29b205893be67ed26b7dbd7a3ebea3038，core固定4866927187e5bcaca4c76238ac12e41a37a922f9。UI隔离分支feat/lean-writing-interactions继续原历史。docs本分支docs/single-window-ui-integration基于设计PR #11；源码功能PR以desktop PR #5分支为基线。

本批支持轻量工作区、五组设置、真实命令工具栏/菜单/快捷键、三内置和用户命名的受控单窗口布局、临时标签和历史定位。维护/身份操作仍可达，编辑器与安全保存保留。右栏默认收起不占布局轨道；中窄窗使用抽屉。

不实现任意文档拆分、多标签组、多视图/独立窗口、AI、同步或出版；不提供假入口。不改变core API、Manifest v3、原生读写、Rust、依赖或构建配置。Session允许改呈现配置的Compartment重设，不改保存/冲突/草稿协议。

## 内部接口与安全约束

新增UiPreferencesV1、LayoutPresetV1和CommandDefinition；统一命令ID、可执行条件与调用入口。单个独立JSON存储快照，版本1，key为collations-ui-preferences-v1。应用偏好与按native工作区路径区分的布局同存一个快照；只存UI，不含正文、草稿或身份。

缺失新记录时读取旧collations-theme，不在读取时写入。损坏/未知版本保留raw与诊断；只有明确恢复并先保全旧记录后才覆盖，保全失败不覆盖。设置编辑先入草稿，校验/保存成功后才确认应用，失败保留草稿和上次确认状态。窄窗适配不覆盖宽屏偏好。

Editor/Surface在设置页、主题和布局切换时保持挂载；光标、选区、滚动、撤销和原始字节行为不得退化。快捷键避免旧监听重复执行，IME和录入/弹窗上下文要区分。临时标签不得替换已改动、恢复或冲突会话。

## 分工与验证

main负责接口/范围、文档和串行集成；ui独占src/**及获准tests/**；regression待固定提交和产物后串行执行；review只读检查，不并发修复。偏好模块确定后再补最终ADR、v1样例和恢复记录，不能用草案冒充实现格式。

纯测试覆盖旧主题迁移、损坏/未知记录、写入/备份失败、预设草稿应用取消、快捷键冲突和历史导航。保留source/link-repair测试及core check/test；最终前端构建、Windows CI原生构建、四项smoke及UI验收分开记录。已知协调os error5须如实对照，不能据其他通过宣称全绿。

真实程序截图覆盖宽屏浅/深、中窗、窄窗，并检查两文档会话状态。模拟视口不等于真实100/125/150%缩放；自动文本输入不等于真人IME。产物记录来源SHA、CI、exe SHA-256，功能PR不自动合并。

## 偏好格式导航

已补 [ADR0007](../adr/0007-ui-preferences.md)及 [v1样例](../examples/ui-preferences-v1.json)，样例经当前desktop validPreferences校验通过。格式和测试结果随最终提交核对；该成绩不等于UI或Windows运行验收。
