# 单窗口 UI 当次程序截图

日期：2026-11-08，Asia/Shanghai。来源为真实 Windows CI exe b7577fc 的 WebView，CDP 模拟视口，并非真实系统 DPI 验收。Figma 新图仍未完成。

exe SHA-256：56026accf13001d35643b25be1879165715188eb86b38908338c8059aafeb630。截图脚本基于0750949并补未提交的CDP简单返回值修正，不将此记录描述为最终提交全绿。

- [wide-light](assets/2026-11-08-single-window-wide-light.png) · SHA-256 `8be78af02f6074716c7d1e814abcea0138b05f67c78d4ec60ce13fcd9b4ee0d2`
- [wide-dark](assets/2026-11-08-single-window-wide-dark.png) · SHA-256 `5a587283014096b150b411cdd0fbdfddebc2db6b6df229b652f32bc083760190`
- [default-inspector](assets/2026-11-08-single-window-default-inspector.png) · SHA-256 `6d234e8e8335b08e3e66ce65d731c23bf7c593edea1414dee484e01c4f39506d`
- [narrow-source](assets/2026-11-08-single-window-narrow-source.png) · SHA-256 `944e5e727b8d8455a109b6e768bd7262e1f1f7c7e8321c3d33d504e0a5890367`

main已视觉检查浅/深/窄图：辅助栏关闭释放空间，窄窗源码入口可见。深色源码链接对比度待修正。当前导航smoke有一次后退超时，诊断运行通过但未关闭竞态排查；不能以截图证明完整交互通过。

四项既有smoke和偏好恢复故障注入于b7577fc当次通过。原UI smoke滚动断言失败，诊断确认外层范围夹紧，修订有效源码锚点后通过该段；后续导航仍需固定提交复验。旧os error 5本次未复现，根因未解决。真人IME、实际DPI、性能与安装环境未验证。
