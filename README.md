# Mewgenics Runtime Research — 结论发布版

**主入口：[coverage-matrix.md](coverage-matrix.md)**

本仓库保存 Mewgenics Bridge 的 Player-View / Runtime 数据来源研究结论。审计基线为 [`Lqrds/mewgenics-bridge@d1edf28d7bd09dab7e051763aaf642f33a650a75`](https://github.com/Lqrds/mewgenics-bridge/tree/d1edf28d7bd09dab7e051763aaf642f33a650a75)，基线提交时间为 **2026-09-15 10:22:10 UTC**。

这是此前独立研究成果的结论发布版，按用户指定写入 `Lqrds/1234`。不是 Bridge 源码镜像，也不是运行时插件；没有修改主项目、启动或操作游戏、加载 MOD、实现 Agent 或发送输入。保留仓库原有 `.gitkeep`。

## 核心结论

**后续应改为“复用现有 Native 机制、迁移已有 RPM 数据源、补剩余语义”，而不是继续大范围从零逆向。**

新的生产代码已经有当前技能 owner/slot/menu/双 callback 绑定、完整当前 open-tooltip 文本、部分当前有效射程/距离/相对 AoE、限定生命周期与实例身份，以及地图 current/visited/coins 的实现。项目自己的 RPM 目录还有地图图边、事件选项 label、商店 price/stock、LevelUp 候选 name/description/affected cat、当前 party 装备与 bag 列表。[生产证据](evidence/production-audit.md)

但必须分开回答三个问题：**已有读取实现吗？它进入默认 Native 路径了吗？它已经成为有资格公开的 Player-View 字段了吗？** 三者的答案经常不同。目录名 `runtime-win32` 不代表默认仍走 Python；实际 `createRuntimeReader` 默认分流到 Native，RPM 只在明确选择诊断后端时启用。

## 1. 哪些只需要搬运、适配或验收？

|已有资产|后续工作，而非重复逆向|
|---|---|
|社区 GON/GPAK、mgmp guarded-read/RTTI、MewUI 字符串/组件片段|沿生产已保留的实现和 provenance 使用，不再造解析器或重选加载路线|
|Native actor/ability/slot/CombatMenu/双 callback 关联|对未支持 UI、redirect 和显示覆盖补资格，不重建槽位系统|
|Native Button mana、tooltip 标题与完整可见文本|扩展覆盖与字段关联；无合格当前显示时保留 null|
|部分 current cache 范围、距离、direction/ray 与相对 AoE|核验现有支持形态，再新增未支持消费者；不把几何形状当最终合法目标|
|Native Character HP/MP/move/attack 原始读取|move/attack 的公开资格尚缺，不是地址未知|
|Native MapMarker/current/visited/coins|补地图边、标签、锁定和公开可选语义|
|Scene/Character/Map/Node incarnation、失效 Hook 与 Director 完成检查|核验实际安装配对和新增域覆盖，不再整体重设计 epoch|
|RPM map/event/shop/reward/party/bag 读取器|迁移已有只读源到 Native 并补当前 owner、寿命与公开语义|

这里“可搬”的是**限定实现片段**，不表示全部技能、敌方、地形或界面已经完整解决。对当前任务，最佳现成来源往往已经是 Bridge 自己的 adapter 或诊断 reader。[不应重复逆向清单](baseline/do-not-reverse-again.md)

## 2. 哪些仍需独立 Runtime 逆向？

|范围|真正剩余的缺口|
|---|---|
|地形与格规则|terrain type、passability、cell movement cost、LOS interaction、enter/stand effects、interactable objects 与格局部规则；复用已有 current grid 根|
|原版运行时状态/被动|完整当前集合、具体 stack/duration 语义、显示过滤与销毁关系；CatData keys、CSF sidecar、tooltip 文字均不能替代全套状态实例|
|技能动态规则|其它成本、charges/uses/reload/共享组、非零 N、自定义/shotgun 等未审消费者、当前 LOS 与几何形状之后的限制|
|地图公开语义|locked、legal-next、真正可见的节点名称与 metadata；内部图边和 destination 不能直接公开|
|事件公开语义|最终标题/正文、公开条件和检查、奖励提示、普通 UI 已明示的结果/转场类型；不能提前执行事件或读取隐藏结果|

## 阅读导航

- [全部 88 行覆盖矩阵](coverage-matrix.md)：保留 G001–G088、A/B/C/D/E、来源层、证据与下一步。
- [新生产路径地图](baseline/path-map.md)、[相对第一轮的修正](baseline/changes-from-first-audit.md)。
- [30 条生产源码定位](evidence/production-audit.md)、[14 个社区来源](evidence/community-sources.md)、[副作用与易误读点](evidence/hazards-and-conflicts.md)。
- [验证等级与边界](evidence/verification-protocol.md)、[许可登记](LICENSES/README.md)、[发布说明](publication.json)。

## 数量不是完成率

原研究覆盖 **10 个域、76 个需求字段、88 个分层判定项**。重新基线化后的分类为 **A=49、B=1、C=23、D=14、E=1**，其中 **29 行仅在 RPM 诊断源中已有，8 行是 Native 私有来源**；47 行分类相对第一版改变。

原研究有 75 条证据卡，其中 30 条是生产源码记录。本发布版保留结论矩阵、生产源码定位和社区审计范围，**不是此前 71 文件研究包或 Git bundle 的逐字镜像**，也没有把它们的全部机器数据、测试代码和历史快照上传到这里。

## 验证状态

本轮结论依据固定提交的源码检查。没有取得实际安装 DLL/Reader/SDK/profile 配对、运行中游戏和原始生产验收记录，因此不授予 `signature_matched`、`runtime_verified` 或作为端到端验收等级的 `bridge_integrated`。**不应因为没有本轮实机验证，就把源码中已经存在的实现重新写成“没有做”。**

社区材料沿用已审固定提交，不声称本次完整重审了所有最新 HEAD。Pilout 未取得可审源码/SDK，保留有价值线索但不按 README 宣称判为已实现。

本仓库公开发布原创结论、结构事实和来源定位；不包含主项目实现、生成的私有 profile、游戏资源或实机记录。主项目链接可能需要授权访问。根 LICENSE 仅覆盖本仓库原创内容，不重许可任何上游或游戏资产。
