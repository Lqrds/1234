# 副作用、反证与易误读点

以下是审计与复用边界，不代表本轮实机复现了全部问题。固定来源见[生产源码](production-audit.md)与[社区来源](community-sources.md)。

## 不可作为纯查询照搬的社区路径

**mgmp 范围预览并非纯绘制。** `mgmp_aim.cpp/.h` 记录部分高亮调用会写状态，有的移动预览会临时挪动角色并恢复；RNG fence 也不能证明没有其它状态副作用。本研究不主动调用这些函数。[mgmp](community-sources.md#mgmp)

**已提交 decision 不等于当前 selection。** PlayerBrain 当前 selected/target/direction 与 cached decision 所处阶段不同。不要用提交到执行之间的短暂状态判断玩家是否正在瞄准。

**Inventory 同步不是只读物品列表。** mgmp invsync 主动驱动序列化并在底层截获 blob，不能因为模块能同步背包，就把它写成无副作用逐项查询。新生产项目已有 read_party/read_bag 只读路径，应优先迁移。[BR-RPM-PARTY](production-audit.md#br-rpm-party)

**Character 离场 sentinel 与成员关系要区分。** mgmp 对 (-5000,-5000) 和已离开实时 roster 的对象设独立限制；盲调依赖 grid 的 affecting-elements 路径可能越界或读取不可靠数据。冻结快照的成员数不等于当前 roster 成员不变。

**元素高位读取缺陷。** ElementalTooltipsExpanded 三个 uint32 值扩成 uint64 再 OR，结果高 32 位仍为零，不能覆盖其 32–37 位表项。CSF 的部分元素位仍有推测命名，不能把两边标签强行合并为真值表。[来源](community-sources.md#elemental-tooltips)

## 新生产代码与历史文档的冲突

|位置 / 易误读点|当前应采用的判断|
|---|---|
|旧 README / handoff 说 Native 输入全 blocked、无生命周期|Reader 已有条件资格与寿命链；本研究既不说始终不可用，也不宣称当前动作验收通过|
|旧 ability-rule-source-slice 说 Native 不提供实例字段|current_abilities 与 abilities.mjs 已提供限定链；不能再以旧句子作为缺口|
|旧说明说 effective rules 全 null|current_effective 和 v2 投影已有部分支持；LOS 和未审分支仍未知|
|旧说明说 ability collector 无分配|当前 AbilityWatch 有有界 heap/index 分配，不沿用旧承诺|
|旧说明把距离/AoE 全部列为未完成|现代码已有 Manhattan reference、ray 与 10 种 mode 子集；不代表 custom/限制后几何已完成|
|acquisition.unavailable 固定出现 ability_text/cost/target_rules|应查实际字段与 guards，不能仅靠汇总标签判断某个值必为 null|
|control-reference 存在|不代表 toolbar/tooltip 的名称、费用和状态已显示|
|unit.description 的 slot 关联技能|不等于所有 ability.description 都填满；effect panel 文字不是 statuses 列表|
|Native roster 读到 move/attack|公开仍 null 是公开资格缺口，而不是 offset 发现缺口|
|map.py 读到 edges/destination|内部图不等于可见路线、锁定或 legal-next；destination 可能泄露隐藏内容|
|Python 注释称 native structures|仍是 RPM 后端，不因此进入默认 Native Reader|
|提交说明含 installed production source|源码同步不等于原始 captures、二进制配对和当前实机验收已提交|

## 必须保留的分界

静态资源存在 ≠ 当前实例已绑定；当前绑定代码存在 ≠ 本轮观察到了实例；已有读取器 ≠ Native 默认支持；Native 私有值 ≠ 已资格公开；当前相对 kernel ≠ 最终合法目标；同一地址 ≠ 同一 incarnation；缺少实机验收 ≠ 实现不存在。

新有效规则 v2 可以接受独立证明、不同于静态声明的当前值，不能继续统一套用旧 v1 的静态相等规则。[BR-JOIN](production-audit.md#br-join)

[主矩阵](../coverage-matrix.md) · [验证等级](verification-protocol.md)
