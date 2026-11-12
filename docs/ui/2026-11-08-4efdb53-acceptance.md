# 单窗口工作区：4efdb53 Windows 验收

测试日期：2026-11-08；归档日期：2026-11-12（Asia/Shanghai）。功能提交：4efdb535f8e7fe138a1877fe49b46788646821ff。

[桌面 PR #7](https://github.com/Tsi-sheng/collations-desktop/pull/7) · [Windows CI](https://github.com/Tsi-sheng/collations-desktop/actions/runs/42486400956) · [x64 Release exe 归档](https://github.com/Tsi-sheng/collations-desktop/actions/runs/42486400956/artifacts/13619891878)

- ZIP SHA-256：b472b165549b472218fa668076dbc186025c13bf5ccbd40f902f9825623b6307
- exe SHA-256：102b3b3d85a4027b215b3c3d39699c0841a7d151e793798e7b1ef13932295d00
- 归档到期：2027-02-06T13:44:41Z。

34 项前端纯测试、TypeScript 与独立目录前端构建通过；Windows CI 的依赖安装、Release 构建和 Rust 测试通过。程序回归使用固定源码、已核验的 CI exe 和复用的 core 样例依赖，不计为本机干净安装。

| 程序命令（依序运行） | 结果 |
|---|---|
| node tests/windows-smoke.mjs | 通过 |
| node tests/reconciliation-smoke.mjs | 通过；旧 os error 5 本次未复现，根因仍未解决 |
| node tests/editor-smoke.mjs | 通过 |
| node tests/navigation-smoke.mjs | 通过 |
| node tests/ui-workspace-smoke.mjs | 第 72 行联合断言超时：已返回证明标签，选区与三层滚动快照未同时满足；原失败保留 |
| node tests/preferences-recovery-smoke.mjs | 通过 |
| node tests/navigation-fast-smoke.mjs | 通过 |
| node tests/workspace-visual.mjs screenshots | 通过，生成四张真实 WebView 截图 |

下载未完成时误启动的 ENOENT 批次没有启动程序，不计作产品结果。后续等待下载退出、摘要校验和解压完成后才得到上述结果。

截图使用模拟 CSS 视口。已检查四种布局及深色链接效果；Figma 新图、真人 IME、实际 100/125/150% 缩放、性能和安装环境未验收。UI 综合 smoke 尚未全通过，PR 保持草稿。

- [wide-light](assets/2026-11-08-4efdb53-wide-light.png) · SHA-256 bef47165b9ba2ae617e2ec34bd99984f66fcfd495bfa6fb06c43a16efabb33e6
- [wide-dark](assets/2026-11-08-4efdb53-wide-dark.png) · SHA-256 876fa48a0b5ed65150a9c74e9b87b0ea9d398f07125226460cd08c875c2ca53a
- [default-inspector](assets/2026-11-08-4efdb53-default-inspector.png) · SHA-256 1d790b7673c8d57906433022f22eda2c3369d549dd2a08b5db85f10ade539fff
- [narrow-source](assets/2026-11-08-4efdb53-narrow-source.png) · SHA-256 5ae146a10f91cf4ea22807989204e989b1ae1b31ca4971d13bf3de77412d6425

## 第 72 行诊断

独立诊断未改变 smoke 结果。测试设定起点后即时采样为横向 1123.2 px、外层纵向 1448.8 px；两帧后、点击定理前已变为横向 64.8 px、纵向 3432.8 px。第二次后退后 200、500、1000 毫秒，均恢复到同一文档、同一选区 37880:37880，滚动也与点击前的稳定样本一致。即时样本不能作为这次导航的有效基线；需用稳定起点重跑，保留非零滚动断言。此诊断不计作程序 smoke 通过。
