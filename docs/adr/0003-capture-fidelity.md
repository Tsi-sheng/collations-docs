# 0003 OCR 草稿的来源、确认与公式保真

状态：已接受。适用于 Capture、Repository 和 Composition。

在 [Latexify-agent 提交 6e30141](https://github.com/yuxuanli2953/Latexify-agent/tree/6e30141e9999e5f7163ff5e14ba311bb4f364804) 中未发现项目级许可证。Lucide 图标文件的许可仅覆盖该组件。Collations 不直接复用、改编或分发上游源码，仅参考可替换视觉模型、保存原始识别结果、独立生成渲染预览三项思想。

审计直接运行了该提交的渲染函数。对照 HTML 中的复制片段，`f(x)` 被截为 `f`，显式公式标签 `\tag{A}` 被移除。分别传入裸 `equation` 和裸 `aligned` 环境时，两者均触发 `IndexError: no such group`。这些结果说明预览、复制和存档应各自验证；测试未测量视觉模型识别准确率。

采用[四层 Capture 模型](../CAPTURE.md)。只有用户确认的内容可进入正式版本和出版。每次修正保留来源与差异，代价是需要保存原始响应和校对历史。按[独立测试规范](../testing/CAPTURE.md)验收。
