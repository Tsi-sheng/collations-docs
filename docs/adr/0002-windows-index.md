# 0002 Windows 工作区身份和派生索引

collations-desktop 提供 Tauri、文件选择、SQLite 与 Windows 文件身份。collations-web 负责浏览器客户端。两个平台共用 core 的扫描和身份匹配逻辑。

Manifest v2 在 v1 的 id、path 上增加 sha256 和可选的 fileKey。迁移保留所有 UUID，并在替换清单前备份 v1 原始字节。块地址由文档 ID 和块锚点共同确定。显式提交才产生 Revision ID；扫描只计算源字节的 SHA-256。

同一路径编辑保留身份。跨路径匹配要求唯一、可靠的文件身份；路径交换、硬链接或缺失旧文件和未登记文件之间有歧义时，返回诊断并保留上次完整结果。缺失文件的身份登记保留。Hash 相等不能证明是同一文档。

一次扫描在持锁的工作区内完成，校验枚举及读取结果后比较 Manifest 原始版本，原子替换清单，随后事务更新 SQLite。索引失败可重试。删除 SQLite 后用 Markdown 和 Manifest 重建。清单丢失时需要从备份恢复才能重现原 UUID。
