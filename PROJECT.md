# 秩笺 Collations

秩笺是本地优先的学术知识工作空间。第一轮交付供协议评审使用。可从[数学样例](https://github.com/Tsi-sheng/collations-core/blob/2ac06c13549469b57696ec99e33056ad051ff9f9/examples/analysis/定理/完备性.md)阅读定理，沿链接查看证明，再对照[讲义清单](https://github.com/Tsi-sheng/collations-core/blob/2ac06c13549469b57696ec99e33056ad051ff9f9/examples/analysis/讲义.yaml)检查出版顺序。

- [产品边界与路线图](docs/ROADMAP.md)
- [架构及平台规范](docs/ARCHITECTURE.md)
- [数据、链接、版本和汇编协议](docs/PROTOCOL.md)
- [身份、索引及版本决策](docs/adr/0001-identity.md)

从上述 core 版本根目录运行 `node --test tests/protocol.test.mjs` 校验样例。

## Capture 架构与验收

- [ADR 0003：OCR 草稿的来源、确认与公式保真](docs/adr/0003-capture-fidelity.md)
- [Capture 数据与确认流程](docs/CAPTURE.md)
- [Capture 独立回归测试规范](docs/testing/CAPTURE.md)
