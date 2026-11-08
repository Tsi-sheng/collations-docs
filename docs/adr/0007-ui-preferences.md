# 0007 单窗口 UI 偏好与布局

状态：本批实施决策；最终运行验证另行记录。2026-11-07，Asia/Shanghai。范围见 [实施约定](../specs/single-window-ui-integration.md)。

## 决策

前端新增 UiPreferencesV1、LayoutPresetV1、CommandDefinition，core与原生接口不变。偏好快照仅存本机localStorage，主key为collations-ui-preferences-v1，version为1。布局按原生返回的工作区root字符串区分，不另造Document ID或修改Manifest。此格式不用于同步，也不包含正文、草稿或引用身份。

app保存theme、fontFamily、fontSize、lineHeight、readingWidth、lineWrapping、reducedMotion、defaultMode、temporaryTabs、toolbar和keybindings。workspaces保存各root的layout和presets列表。layout包含mode、sidebarVisible/sidebarWidth、inspectorVisible/inspectorWidth/inspectorTab、splitRatio；用户预设为id/name/layout。具体合法范围和校验由desktop集中模块执行，文档样例须同时通过该校验。

三内置预设不可直接覆盖；用户预设使用独立ID，非空规范化名称不得与内置或用户项重复。源码/预览比例与侧栏宽度是显示偏好，不涉及文档字节。任意拆分、多标签组及独立窗口不在v1实现范围。

## 迁移与恢复

新主key缺失时读取旧collations-theme中的light/dark/system，得到内存默认偏好，不在读取时写入或删除旧key。用户应用设置时才保存v1快照。读取遇到损坏JSON、未知版本或校验失败，保留原raw并报告；仅用内存默认值继续查看，不自动覆盖。

正常保存先校验整份快照，存储成功后才确认应用，失败保留编辑草稿和上次确认值。显式恢复前需确认操作范围，先将损坏raw写到collations-ui-preferences-v1-backup并回读核对；备份写入/核对失败时不得覆盖主key。备份key保存最近一次显式恢复所保全的raw，不宣称是多版本归档。恢复偏好不清除Markdown、Manifest、SQLite或恢复草稿。

使用单一快照提交应用偏好和工作区布局/预设，避免逐字段保存形成半套配置。本批不是跨窗口事务协议，不承诺处理未来多窗口并发写偏好；新增窗口前另行评审。设置预览与布局草稿可以临时改变呈现，取消回到已确认状态，不重建CodeMirror。

## 编辑器与命令

Editor/Surface按会话保持挂载。字体和自动换行使用CodeMirror Compartment重设，不以重建EditorView达到换主题/布局效果。统一命令定义供工具栏、菜单、命令面板和快捷键复用；自定义快捷键应用前检查冲突，上下文与IME优先于不适用的命令，不与旧全局监听重复触发。

临时标签仅替换未修改、未恢复、无冲突的临时会话。历史记录文档、选区与视图位置，失效目标不能绕过既有身份/文件校验。本批不改原生保存与草稿协议。

## 样例与验收

[版本1样例](../examples/ui-preferences-v1.json)使用合成工作区路径。desktop测试覆盖旧主题读取、正常保存、损坏/未知版本、备份失败/回读失败、存储失败、预设和快捷键。程序验收需证实切换设置/模式/主题/侧栏后会话实例、文本、撤销、选区、滚动保持；当次结果和失败必须另附，不以本ADR代替测试成绩。

## 2026-11-08 审查补充

快捷键覆盖须同时阻止 CodeMirror 的旧应用命令绑定，不能仅增加新的全局入口；普通文本编辑与 IME 仍由编辑器处理。窄窗模式是视口生命周期中的临时选择，回宽屏恢复已确认布局，无关主题设置不得重置窄窗选择。首次建立工作区布局记录以当前有效布局为起点，不以静态默认值覆盖应用偏好。

导航记录应覆盖选区和实际滚动容器的纵向、横向位置；同文档锚点往返也适用。偏好恢复的成功或失败必须在操作当时可见，备份失败继续保留原始记录。打包测试须先通过可见入口展开折叠面板，不能依赖隐藏 DOM 操作。上述要求的最终测试结果另列于当次验收记录，不以设计决策冒充实际通过。
