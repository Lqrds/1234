# 相对第一轮的修正

生产基线：`d1edf28d7bd09dab7e051763aaf642f33a650a75`。第一轮独立研究提交：`836d59143ed04d07c61bb552dd764f6173d4259d`；重新整理后的本地研究提交：`0e360fa59d3d85e9016a661a2b7815411718c9ee`。这些是历史研究标识，不是本公开仓库的提交 ID。

**变化不是社区突然逆完了，而是新生产源码已有很多第一轮未计入的实现。** 社区片段及副作用结论继续保留，剩余缺口按实际生产路径重算。

|旧判断|应采用的新判断|生产证据|
|---|---|---|
|只有静态技能，缺当前绑定|已有 owner/slot/双 callback/菜单 payload/渲染链和 Reader 身份 join|BR-ABILITIES、BR-ABILITY-READER|
|动态射程 / AoE 整体仍需从零做|标准 current cache、距离、direction/ray 与 10 种 mode 已有；未支持分支才是新增 RE|BR-EFFECTIVE、BR-EFFECTIVE-PROJECTION|
|当前 tooltip 正文没有来源|已有完整 open-tooltip visible-text-nodes，含单位、技能和附加面板|BR-DESCRIPTION、BR-TEXT-CONTRACT|
|mana / attack 字段未知|Native 内部已有 MP/maxMP/move/attack；后两项公开仍 null|BR-ROSTER、BR-PUBLIC|
|地图 current / visited 仍需逆|Native marker 关系及 visited/coins 已连到 public|BR-MAP|
|地图边、商店价格库存、升级文字都缺|RPM 已有读器，应做迁移与显示资格|BR-RPM-MAP、BR-RPM-SHOP、BR-RPM-CONTEXT|
|当前装备只能从 save 读|read_party/read_bag 已有当前冒险动态关系|BR-RPM-PARTY|
|生命周期端到端整体未实现|限定对象的失效 Hook、incarnation、Director 完成检查及 Reader proof 链已经存在|BR-LIFECYCLE、BR-INCARNATION、BR-DIRECTOR、BR-READER|
|全部 Native 输入始终不合格|新 Reader 有条件资格分支；本研究不调用或验收它|BR-READER、BR-PUBLIC|

[生产证据 ID 索引](../evidence/production-audit.md)

全部 88 行保留 G001–G088，其中 47 行 A/B/C/D/E 分类发生变化。A 增多不能算成功率：它可能只表示诊断代码已有，或者只覆盖有限当前公开子集。

## 仍需独立研究

地形/格级规则，完整原版状态与被动集合和 stack/duration，其它费用与 charges/reload，非零 N、自定义/shotgun 消费者、当前 LOS 与 kernel 后限制，地图 locked/legal-next/可见 metadata，事件公开条件/提示/已暴露结果类型。

单位显示名、阵营、shield、action counters、装备和旧界面数据中，许多已有原始字段或读器。优先补 Native 来源与公开资格之间剩余的一段，不重复做字段发现。

[返回主矩阵](../coverage-matrix.md)
