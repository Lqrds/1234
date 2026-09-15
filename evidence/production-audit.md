# 生产源码证据索引

本页对应矩阵 P01–P30，全部固定到主项目 `Lqrds/mewgenics-bridge@d1edf28d7bd09dab7e051763aaf642f33a650a75`。源码为私有仓库，链接需要相应权限。本页只保留定位、结构事实和原创结论，不复制实现。

统一状态：`source_confirmed`；当前目标 binary/profile 未独立匹配，未做本轮实机验证。`source_path_present` 只表示源码链存在，不是端到端 `bridge_integrated` 验收。原始证据中的 blob SHA 和阅读范围在此保留；“全文”指取得的文件全文，不代表全仓库审计。

## BR-ENTRY

P01 · [packages/runtime-win32/index.mjs][P01] · `createRuntimeReader`。

Blob：`232a27a9c2c5184f755a66c1aee9696b5c1b837f`；范围：全文。

没有注入测试 transport、也没有显式 `rpm-diagnostic` 时返回 `createNativeReader`。RPM 不是静默 fallback。旧 Python 文件是可迁移资产，不证明默认 Native 支持。

## BR-SDK

P02 · [packages/sdk/index.mjs][P02] · `createGame`。

Blob：`437ff0cb83ef0b1e42200392ceedf3e5a1e3c800`；范围：1–90 行。

SDK 接收 backend/catalog，并向 Reader 与 Core 传递。这里只确认接线，不确认实际安装或所有启动自动加载 catalog。

## BR-ROSTER

P03 · [src/battle_roster.inc][P03] · `collect`、`battle_identity_still_qualified`、`battle_component_state`。

Blob：`e480d398f62739672c1ee80b6c53120647f35795`；范围：全文。

从当前 scene 注册表关联 Character/Brain/Tactics/Grid，并做回指、同帧及删除状态检查。已读 HP/maxHP、MP/maxMP、move/attack 和位置。MP 位于 +0xD18/+0xD1C，move/attack 位于 +0xD10/+0xD14。原始值不自动获得公开资格；roster 也不等于全部可见单位或 party。

## BR-ABILITIES

P04 · [src/current_abilities.inc][P04] · `AbilityWatch`、`ability_display_capture`、`ability_tooltip_name`、`acquire_current_abilities`、`collect_current_abilities`。

Blob：`c6e8ac24fd2aacd58119399d602a9d6cbaadc24d`；范围：1–115 行、443 行至文件末尾。

当前 Character 槽位、PlayerBrain、选择 callback、独立 display callback、CombatMenu payload/index 与 renderer 关系组成有界绑定。Button mana 与当前 tooltip 标题可供显示事实。没有合格 toolbar/tooltip 时不能填名称或费用。AbilityWatch 当前有有界 heap/index 分配，不能沿用旧文档“完全无分配”承诺。

## BR-ABILITY-READER

P05 · [packages/native-backend/abilities.mjs][P05] · `validateCurrentAbilities`、`projectCurrentAbilities`。

Blob：`67188e23e76c85dbe7f0e27c04ff45652bcf24f4`；范围：全文。

闭合验证当前 UI rows，独立 definition key 和 owner/sample 绑定；以 actor、来源、定义和槽位生成 selector 身份。selector 不是 Ability/Button allocation 永久 ID；绑定每个 sample 重新成立。代码存在不代表所有技能均有当前显示证据。

## BR-EFFECTIVE

P06 · [src/current_effective.inc][P06] · `effective_source_current`、`effective_gon_child`、`acquire_effective_rules`、`collect_effective_rules`。

Blob：`4733b567b62e7c2d2b23db728b1d018ccdc0817e`；范围：全文。

核对当前 actor modification epoch 与 Ability cache epoch、限定 loader/refresh/consumer，再读取 target、range、direction ray、footprint 与相对 area。不是主动调用 refresh。非零 N、自定义/shotgun、未审消费者继续缺失；当前 LOS 并未因此解决。

## BR-EFFECTIVE-PROJECTION

P07 · [packages/native-backend/effective-rules.mjs][P07] · `validEffectiveParameters`、`effectiveRulePresentation`。

Blob：`c51ff169852f5c61af6e65dedcfc9dd612c4f698`；范围：全文。

当前参数 v2 可表达 cell/direction/none、Manhattan origin/footprint、方向射线、10 个 area mode（含三种 cone）。`relative_area.basis` 是 `geometric-kernel-before-restrictions`。`line_of_sight` 仍 null；ray 的 stops_on_unit 不等于伤害穿透、terrain LOS 或保证命中。

## BR-INDEPENDENT

P08 · [src/independent_rules.inc][P08] · `seed_independent_rules`、`independent_selection`、`acquire_independent_rules`、`sample_independent_rules`。

Blob：`6a765899d9c72352d3f5371d4a3549433a44d44d`；范围：全文。

额外请求在正常消费者返回处采样规则，重新检查 owner、selection、寿命和 source。选择字段区分 selected/action_ability 与 i32 stage/action_type/submitted。不是为了读取而主动执行游戏函数，也不把选择状态直接作为公开 turn/secondary。

## BR-JOIN

P09 · [packages/core/ability-rules.mjs][P09] · `joinAbilityRule`、`confirmedRules`、`abilityPresentation`。

Blob：`e36fa957e828f77c7aa4a9adee1ebce0c02e1a86`；范围：140 行至文件末尾。

检查 source/version/snapshot/definition/actor/ability/slot/owner。旧 v1 的静态相等确认与 current v2 独立证据不同；已证明的 current v2 可保留不同于静态声明的值。源码检查仅覆盖列出的范围，不外推全部 catalog 校验。

## BR-DESCRIPTION

P10 · [src/current_descriptions.inc][P10] · `description_wide`、`description_panel`、`acquire_current_description`、`collect_current_descriptions`。

Blob：`0d9849efb5ac796fc874260f5c70d54727014308`；范围：全文。

复用当前 ToolTip owner、visible tree、MewUI 字符串和 guarded reads，支持技能、CharacterTooltip、LightweightCharacterTooltip_Enemy 及 Status/Keyword 附加面板。完整复制可见文本，保留换行和内联 markup，不返回截断前缀。只覆盖当前打开的合格 tooltip，不是所有物品或所有技能后台文本。

## BR-DESCRIPTION-PROJECTION

P11 · [packages/native-backend/descriptions.mjs][P11] · `projectCurrentDescription`。

Blob：`23e93f5f40eda57653c35eb92534ad6bcde758ff`；范围：全文。

文本绑定同一 snapshot、world、scene incarnation、actor incarnation 和技能槽。通过 unit.description 的 slot 说明技能归属。locale 未推断；文字采样有效性不保证后续持续有效。

## BR-TEXT-CONTRACT

P12 · [packages/core/tooltip-description.mjs][P12] · `publicTooltipDescription`。

Blob：`2624c062a63d31cdb0ffd2dee35857f09d544132`；范围：全文。

闭合公开文本 envelope，区分 unit/ability/effect panels，检查完整字符串及大小边界。effect panels 是文字，不是结构化 statuses/passives，也不能从文字 envelope 宣称机制已全部逆出。

## BR-READER

P13 · [packages/native-backend/reader.mjs][P13] · `validateLifetimeProof`、`validateMap`、`validateNativeSnapshot`、`createNativeReader`、`currentRuleSelectionEligible`。

Blob：`ce441f7b030a7b66416daccd076c94454459c096`；范围：1–240 行响应存在截断；另读 230–365 行，不宣称无遗漏全文审计。

可见代码核对 Native snapshot、nonce/freshness、生命周期 proof，并为 map/character 生成作用域 ID。DESCRIPTION_SNAPSHOT 是完整更新帧，不拼接旧帧。仅接受 Battle/Map 事实场景。存在条件输入资格分支；本研究不调用它，也不以此确认当前任何动作可用。

## BR-PUBLIC

P14 · [packages/core/player-view.mjs][P14] · `nativeAbilities`、`playerObservation`。

Blob：`a72bbe68cd60d9ebb9542157a9df5cf96e2ab28d`；范围：两段取得全文。

Native public units 按 presented 筛选；原始 roster 中已有字段不一定输出。name、side、shield、move/attack resources、statuses/passives/equipment、cells 以及 secondary 等仍有明确 null/unavailable。map 只发限定字段。固定 unavailable 标签不能当作每个实例实际值的精确反向索引。

## BR-MAP

P15 · [src/map_observation.inc][P15] · `collect_map_nodes`、`collect_map_current`、`collect_map_currency`、`collect_map`、`map_response`。

Blob：`aa082c9dbc6b09abcd0e263c7d284141896f5d58`；范围：全文。

当前 MapScreen/MapMarker、node/button/renderer 归属、visited 和 Inventory coins 有真实读取及序列化路径。node 根、当前节点和币数不等于完整公开图、合法下一节点、锁定或任意前台 overlay 资格。

## BR-LIFECYCLE

P16 · [src/lifecycle.inc][P16] · `Boundary`、`boundary_enter`、`boundary_leave`、`observe_load`、`observe_director_update`、`observe_component_remove`。

Blob：`bab5844a20c234f6ac0a9700c02a271e3d05c19c`；范围：1–170 行。

失效在原函数前发生，入口/出口计数和 finally 防护限制采集时机；Director update 完成后进入采集检查。这里确认已有机制，不确认全部 Hook 安装、原始签名或所有路径覆盖。

## BR-INCARNATION

P17 · [src/lifetime_registry.inc][P17] · `LifetimeKind`、`LifetimeEntry`、`lifetime_admit`、`lifetime_retire`、`lifetime_reset`、`lifetime_registration`。

Blob：`44ca9942c32d04fe99822c2c25479dafb0a860dc`；范围：全文。

固定容量保存 Scene/Character/Map/Node incarnation，退休和 reset 失效，重复/异常注册不能仅凭地址恢复身份。自建 incarnation、arena stamp 和 UI selector 三者不是同一种身份。

## BR-DIRECTOR

P18 · [src/director_completion.inc][P18] · `director_completed_at`、`director_completed`。

Blob：`2d76257593446257caece71d5b0bcfacade81cfb`；范围：全文。

正常 Director update 后检查 pending reset、scene 向量、owner、销毁标志，选择唯一非 paused/parked 的 Battle/Map 候选；观察到候选变化或空缺即失效。已有当前实例机制，不是任意 screen/overlay 的通用完成证明。

## BR-RPM-MAP

P19 · [profiles/current/map.py][P19] · `read_map`、`map_callback`。

Blob：`361d80950da510c27795009ad95f059a18aebed0`；范围：全文。

当前 node+0x140/+0x150 两类边向量、成员校验、kind/type/destination、visited/raw flags、button 与 renderer/camera 路径已有。边容器无需重逆；公开边、锁定和 legal-next 仍须独立语义，destination 可能是隐藏未来内容。

## BR-RPM-EVENT

P20 · [profiles/current/event.py][P20] · `read_event`。

Blob：`65a3f76590a9f92c40ebeb6689522bacf252fb15`；范围：全文。

有界 0xF0 选项数组、+0x20 UTF-16 label、stat、按钮键及 event_name_key/prompt_key 已读。标题和正文键不等于最终 UI 文本；battle_trigger 仍未验证。读取器只在诊断后端，不是 Native Event frame。

## BR-RPM-SHOP

P21 · [profiles/current/shop.py][P21] · `read_shop`。

Blob：`b3e05eff6c544bbd4acbea2fc9a744bec900be49`；范围：全文。

Shop 的 0xC0 slots 包含 item keys、price、remaining_stock；售罄诊断取 stock<=0，并有不同货币源。price/stock 不必重找。售罄不证明购买，affordable 是派生诊断；非关闭 Shop 仍可能在模态窗口后。

## BR-RPM-INVENTORY

P22 · [profiles/current/inventory.py][P22] · `read_inventory`。

Blob：`c6659232fedf353aa04caa852e22fc4c0d5330f8`；范围：全文。

唯一 InventoryScreen2、box 向量、screen owner、button/renderer/camera、selected key 与 bag size 已读。UI key 不是完整 Equipment 语义或可装备授权；旧几何检查不能自动替代新 Native compositor 资格。

## BR-RPM-PARTY

P23 · [profiles/current/native.py][P23] · `Watched.linked_map`、`read_character_life`、`read_party`、`read_bag`。

Blob：`022a4fb022330538d83e64ab90760339415eee3c`；范围：全文。

当前冒险 party keys→CatData，五槽装备、active/passive keys、bag linked-map 已有只读关系；Character shield/death 也有独立绑定。CatData 不是任意敌方当前状态，record ID 不保证跨进程持久；这些大多未进入 Native 默认公开字段。

## BR-RPM-CONTEXT

P24 · [profiles/current/context.py][P24] · `Context.characters`、`Context.reward`、`Context.read`。

Blob：`4c9fe8602381b1feb859efb1c14c1669cf6c470e`；范围：1–175 行、240 行至文件末尾。

诊断层已读 Character faction/label/class。LevelUpScreen 有 name/description/kind/key、affected CatData、committed 与按钮 owner/index 关联。该 reward 特指 LevelUpScreen；宽字符串不自动是当前实际渲染，faction 数值意义仍需验证。

## BR-RPM-MENU

P25 · [profiles/current/menu.py][P25] · `read_menu_buttons`。

Blob：`4b22102cff02922d4831d0d21b6946fed636491d`；范围：全文。

MenuPanel 按钮树、renderer/camera 和 LevelUp lambda owner/index 解码已有。callback 身份和几何不证明全部 selectable 条件；仍是 Python source-specific reader。

## BR-UPSTREAM-PINS

P26 · [packages/native-backend/provenance.json][P26] · `sources`、`files`、`mgmp.commit`。

Blob：`10044c9690e590efd389d15c862677ed4f5dda31`；范围：1–160 行。

Mewjector/MewUI 沿原审计 pin；生产 mgmp 为 `16cc79cfa48f41cef2e9afdcd1f72550a18db09b`，研究版为 `bcd9985bcf7b388df1dd03006cd66fc944d91015`。列出 guarded read/RTTI 等实际 helpers，不证明整个 mgmp 系统被移植。未重新计算每个 vendor 文件字节哈希。

## BR-RULE-PINS

P27 · [packages/rules-source/provenance.json][P27] · `sources.gon`、`sources.gpak`、`adapted_files`。

Blob：`6ae4462d532feea456fbfedadf97a370277a63c5`；范围：全文。

已记录 GON/Tiftid 原始与改编文件哈希、受限提取和 tokenizer 适配。此记录不是本轮重新构建、提取或读取游戏目录的证明。

## BR-EFFECTIVE-TEST

P28 · [packages/native-backend/tests/current_effective.cpp][P28] · `main`、`read_rules`。

Blob：`75eaff496bee2b7313f571c6289691ca640b0745`；范围：全文，仅阅读。

合成测试确实调用实际 acquire_effective_rules/serializer，覆盖 epoch/N/method/owner/shape/cache 变化、零值和动态边界。**本轮没有编译执行，不算 runtime_verified，也不是全 Native 测试套件审计。**

## BR-RULE-COMPILER

P29 · [packages/rules-source/native/gon_rule_catalog.cpp][P29] · `DeclaredRule`、`select_rule`、`resolve_definition`、`write_catalog`、`main`。

Blob：`01887aa76b97636b71938c23d7cdd76c534c5183`；范围：全文。

实际调用 vendored GonObject::Load/DeepMerge，处理 template/variant 的有界继承、重复与循环检查，输出 literal target/range/explicit LOS。不是完整费用、名称、文本编译器或表达式求值器；静态输出不供应当前有效默认值。

## BR-RULE-LOAD

P30 · [packages/rules-source/index.mjs][P30] · `loadAbilityRuleCatalog`。

Blob：`6d6e1f6976e3744c6777e1cb2f0e37863fc95cf5`；范围：全文。

显式文件 loader 有 16 MiB 上限、受限流读取、严格 UTF-8 JSON 和 catalog 校验。只证明可加载指定目录文件，不证明每次默认启动都有合格目录；本轮没有实际加载游戏数据。

[主矩阵](../coverage-matrix.md) · [社区来源](community-sources.md) · [验证模型](verification-protocol.md)

[P01]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/index.mjs
[P02]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/sdk/index.mjs
[P03]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/battle_roster.inc
[P04]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/current_abilities.inc
[P05]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/abilities.mjs
[P06]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/current_effective.inc
[P07]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/effective-rules.mjs
[P08]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/independent_rules.inc
[P09]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/core/ability-rules.mjs
[P10]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/current_descriptions.inc
[P11]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/descriptions.mjs
[P12]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/core/tooltip-description.mjs
[P13]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/reader.mjs
[P14]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/core/player-view.mjs
[P15]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/map_observation.inc
[P16]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/lifecycle.inc
[P17]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/lifetime_registry.inc
[P18]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/src/director_completion.inc
[P19]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/profiles/current/map.py
[P20]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/profiles/current/event.py
[P21]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/profiles/current/shop.py
[P22]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/profiles/current/inventory.py
[P23]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/profiles/current/native.py
[P24]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/profiles/current/context.py
[P25]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/runtime-win32/profiles/current/menu.py
[P26]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/provenance.json
[P27]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/rules-source/provenance.json
[P28]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/native-backend/tests/current_effective.cpp
[P29]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/rules-source/native/gon_rule_catalog.cpp
[P30]: https://github.com/Lqrds/mewgenics-bridge/blob/d1edf28d7bd09dab7e051763aaf642f33a650a75/packages/rules-source/index.mjs
