# 验证等级与发布边界

## 两个独立维度

**来源接入状态**记录源码事实：静态资源、社区片段、RPM 诊断已有、Native 私有源、Native 有限公开子集、Native 有界实例链。必须检查实际 reader/collector/projection，不能只引用 README。

**证据验证等级**统一使用：

|等级|含义|
|---|---|
|documented|有可定位文档说明，不自动等于实现|
|source_confirmed|已在源码确认限定实现或字段使用|
|structure_confirmed|已确认限定结构定义；不能扩展到未知成员|
|signature_matched|已对指定目标二进制完成对应签名匹配|
|runtime_verified|在指定运行时和支持形态中实际验证|
|bridge_integrated|目标环境端到端接入验收，不只是源码出现 import|

本次生产源码记录为 `source_confirmed`。原社区记录中可能有 `documented` 或 `structure_confirmed`。本轮不授予后三个等级；“源码路径已接入”和“端到端验收通过”分开记。

## 没有做过的验证

没有启动游戏、输入、加载 MOD、访问游戏进程、匹配用户当前 exe 签名、调用 preview/getter/serializer 或执行 Bridge 实机验收脚本。没有取得实际 DLL/Reader/SDK/profile 配对和原始生产验收记录。

生产测试文件 `native-backend/tests/current_effective.cpp` 在审计中只被阅读，没有编译执行。旧文档所报告的实机与测试结果不能记作本研究重跑。

## 下一次验证应保留的证据

同一配对 exe/resource/profile 和代码 commit，源字节匹配与唯一性，当前 scene/owner/incarnation，快照 freshness，实际显示与公开字段对应，以及 unknown/0/false/empty 的差异。还应覆盖同址复用、读档、重入、离场、取消选择、插件覆盖等失效条件。

必须按具体字段和支持形态验收，不给整个域笼统通过。没有查到可靠来源，只表示本次审计范围内未找到，不断言社区绝无实现。

## 本公开仓库与完整研究包的区别

此前完整研究包包含 71 个文件、75 条证据卡、JSON Schema 和离线研究校验程序，并附 Git bundle。本仓库是用户要求的**结论发布版**，保留 88 行判定、30 条生产源码定位、14 个社区来源、路径迁移结论和验证边界；不是完整研究包逐字镜像。

完整研究包此前报告的 19 项检查、166 个 Schema 实例检查属于那个包，不能用于声称本发布版已运行同一测试套件。本次 GitHub 写入应另以提交、目录及关键文件回读验证。

不上传生产源码、私有生成 profile、游戏资源、实机 captures、日志或安装二进制。对私有主项目的链接可能需要仓库权限。

[主矩阵](../coverage-matrix.md) · [生产源码定位](production-audit.md)
