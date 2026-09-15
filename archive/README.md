# 版本化研究归档

`main` 的 README、coverage-matrix 和 evidence 页面继续作为结论与证据定位入口。完整研究材料不在 `main` 中展开，不提交拆分的 Base64 文本，也不把 GitHub 自动生成的 Source code ZIP 当成完整研究包。

完整包优先作为 GitHub Release 的三个独立附件保存：版本化 ZIP、版本化 Git bundle，以及对应的 `SHA256SUMS.txt`。后续版本使用新的基线标识，不覆盖已发布文件。

## d1edf28

- Bridge 审计基线：`d1edf28d7bd09dab7e051763aaf642f33a650a75`。
- 完整研究 Git HEAD：`0e360fa59d3d85e9016a661a2b7815411718c9ee`。
- 拟用 Release tag：`research-d1edf28`。
- [文件清单与发布状态](d1edf28/manifest.json) · [SHA256SUMS.txt](d1edf28/SHA256SUMS.txt)。

**此归档索引提交时的状态：本地完整包已验证；Release 附件尚未上传。** 本目录当前只保存归档索引与校验值，没有伪装成 ZIP/bundle 的文本文件，也没有不存在的附件下载链接。发布后以 Release 实际附件与 SHA256 校验结果为准。

本地验证已完成 ZIP CRC、bundle 完整历史检查、bundle 恢复，以及恢复后 71 个跟踪文件与 ZIP 内容的逐字节比对。ZIP 与 bundle 原始字节未重新打包或改写。归档保存的是研究材料，不增加任何实机验证等级，也不是 Bridge 主项目源码镜像。
