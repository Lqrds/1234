# 覆盖矩阵 — 主入口

固定生产基线：[`Lqrds/mewgenics-bridge@d1edf28d7bd09dab7e051763aaf642f33a650a75`](https://github.com/Lqrds/mewgenics-bridge/tree/d1edf28d7bd09dab7e051763aaf642f33a650a75)。审计日期：2026-09-15。

本结论发布版保留原研究的 **76 个需求字段、88 个分层行 G001–G088**；文字压缩，但 A/B/C/D/E 分类和生产来源层与重新基线化的研究一致。**A=49，B=1，C=23，D=14，E=1；不是完成率。**

## 阅读规则

A=已有可复用代码；B=明确的限定结构定义，需适配；C=有不完整线索；D=仍需独立 Runtime 语义取证；E=本次范围内无可靠来源。每一等级仅针对该行，不授予整域完成。

Runtime 列：**静态**=离线声明；**N部分**=Native 已接入有限公开子集；**N私有**=Native 私有源已有，公开仍受限；**N绑定**=Native 有界实例链已有；**RPM**=诊断已有、未迁入默认 Native；**社区**=未证生产接入；**待逆**=剩余语义需独立取证；**无源**=本轮未找到。上述均为源码事实，不表示本次观察到了运行中实例。

验证列的 `SC` 是 `source_confirmed`，`ST` 是 `structure_confirmed`，`DOC` 是 `documented`。**全表当前目标环境均未在本轮进行签名匹配或实机验证；不存在本轮 `signature_matched`、`runtime_verified` 或端到端验收级 `bridge_integrated`。** 不能反过来把缺少本轮实机验收写成源码不存在。

许可：`O`=自有私有主项目实现，未建立第三方公开复制授权；`M`=MIT；`G`=AGPLv3、only/or-later 未确认；`U`=许可未确认；`P`=Pilout 未建立开源授权。每行的来源定位与具体许可见 [生产证据](evidence/production-audit.md)、[社区来源](evidence/community-sources.md)、[许可说明](LICENSES/README.md)。没有复制上游或主项目源码。

生产状态分布：静态 8、N部分 13、N私有 8、N绑定 11、RPM 29、社区 4、待逆 14、无源 1。

## 1. 技能 / 攻击规则

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G001 name / 静态 ID、本地化|GON/定义 ID 与本地化工具已有；生产有目录来源标识|[P27] [P29] [P30] [C11]|A|静态|SC / 未实机|O+M+G|保留解析器；静态译名不替代当前显示名|
|G002 name / 当前实例|owner、slot、双 callback、tooltip 标题有 Native 路径；无合格 tooltip 名称仍 null|[P04] [P05] [P14]|A|N部分|SC / 未实机|O+M|核验配对版本和显示覆盖，不重建 owner|
|G003 cost / 静态声明|费用键有资料，现有目录不是完整费用表达式求值器|[P27] [P29] [C08]|C|静态|SC / 未实机|M+U|新增费用先找当前 UI 来源|
|G004 cost / 当前费用|Button mana 数字可读，MP 零值保留；其它成本仍 null|[P04] [P05]|A|N部分|SC / 未实机|O+M|复用显示链，另证 HP/行动/移动消耗和符号格式|
|G005 description / 静态模板|本地化解析已有，不等于当前完整 tooltip|[C11] [P27] [P29]|A|静态|SC / 未实机|G+M|本地版本资源与动态占位符分开|
|G006 description / 当前 tooltip|完整可见文本及附加面板已有，经 unit.description+slot 关联|[P10] [P11] [P12]|A|N部分|SC / 未实机|O+M|验证 open-tooltip 覆盖，不宣称后台全技能描述|
|G007 target type / 静态模式|authored target_mode 已知；unit/empty/self 仍需限制语义|[P29] [C08]|C|静态|SC / 未实机|M+U|保留 restrictions，不把 tile 一律变成 unit|
|G008 target type / 当前|cell、direction4/8、none 有缓存读取与投影|[P06] [P07] [P09]|A|N部分|SC / 未实机|O+M|沿现有 consumer/epoch 检查扩缺失模式|
|G009 range / 静态声明|继承后的 literal range 已可编译；表达式不是有效值|[P29] [P30] [P09]|C|静态|SC / 未实机|O+M|保持静态声明和当前结果分层|
|G010 range / 当前有效|部分 cell min/max 和 ray reach 已读；有自然消费者后取样|[P06] [P07] [P08]|A|N部分|SC / 未实机|O+M|先验 generic/zero-N，再补 N-dependent/custom/shotgun|
|G011 distance rule|标准 cell Manhattan origin/footprint 和 ray 距离已有|[P06] [P07]|A|N部分|SC / 未实机|O+M|补其它 range_mode；不是地形移动代价已解决|
|G012 LOS / 当前|effective line_of_sight 仍 null，静态声明不能证明当前遮挡|[P07] [P09]|D|待逆|SC / 未实机|O+M+U|定位正常 LOS/restrictions 消费者，不主动调用有副作用预览|
|G013 relative AoE / 静态|custom_aoe、symmetry 有资料，未形成完整 custom 解释器|[C08] [P07]|C|静态|SC / 未实机|U+O|保留原点、方向和对称的未审语义|
|G014 relative AoE / 当前|10 个 mode（含三种 cone）已有 current kernel 投影|[P06] [P07]|A|N部分|SC / 未实机|O+M|只补未支持分支；kernel 是 restrictions 前几何|
|G015 charges / 声明|uses_per_fight、cantrip、reload 等静态键可查|[C08] [C04]|C|静态|DOC / 未实机|U+M|区分次数、耐久、充能和共享组|
|G016 charges / 当前|当前 row/public 未提供完整 remaining/used/group/reload|[P05] [P14]|D|待逆|SC / 未实机|O+M+U|定位当前计数、显示和刷新时点|
|G017 dynamic modifiers|修改 epoch/cache epoch 与 v2 当前值路径已有；完整修正来源未枚举|[P06] [P09]|C|N部分|SC / 未实机|O+M|v2 不要求等于静态；补非零 N 与其它依赖|

## 2. 敌方 / 中立单位

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G018 name|Character label/class 与单位 tooltip 已有；public name 仍 null|[P24] [P10] [P14]|C|RPM|SC / 未实机|O|确认显示语义，不能直接把 class/label 当名称|
|G019 type|common Brain registry/controller class 已读；不是物种或阵营|[P03] [P14]|C|N私有|SC / 未实机|O+M|补公开类型及 props/召唤/中立映射|
|G020 faction|RPM 已读 Character+0x358，native side 仍 null|[P24] [P14]|C|RPM|SC / 未实机|O|验证枚举、临时控制和可见边界后迁移|
|G021 HP|Native HP/maxHP 已读，公开仅 presented/HUD 合格子集|[P03] [P13] [P14]|A|N部分|SC / 未实机|O+M|扩展其它单位展示资格，不直接公开整个 roster|
|G022 shield|read_character_life 有 shield/dead 绑定；public shield 仍 null|[P23] [P14]|A|RPM|SC / 未实机|O|迁移已知读取并补显示资格|
|G023 mana|Native MP/maxMP 采集和 HUD 子集公开已有|[P03] [P13] [P14]|A|N部分|SC / 未实机|O+M|补其它实体的 presentation、unknown/0 验收|
|G024 movement|Native 已读 +0xD10，public resources.move 仍 null|[P03] [P14]|A|N私有|SC / 未实机|O+M|补余量/消耗和显示语义，不重找 offset|
|G025 attack counts|Native 已读 +0xD14，public resources.attack 仍 null|[P03] [P14]|A|N私有|SC / 未实机|O+M|区分回合次数、攻击消耗与额外行动|
|G026 passives|当前 CatData passive keys 与附加文本已有；任意敌方当前集合仍缺|[P23] [P10] [C05]|C|RPM|SC / 未实机|O+M|复用现有来源，补原版当前枚举与显示过滤|
|G027 statuses / 结构片段|CSF Passive 字段和 custom sidecar 可适配|[C05] [P10]|B|社区|ST / 未实机|M|接具体实例/owner/寿命，未知成员保持未知|
|G028 statuses / 完整当前集合|部分附加文字可读；结构化 statuses、层数和时效未闭合|[P10] [P14] [C05]|D|待逆|SC / 未实机|O+M|定位集合、具体 stack/duration、销毁和可见性|
|G029 visible intent|没有可靠的 UI 明示意图来源；不能读取 AI decision 替代|[P14] [C03]|E|无源|SC / 未实机|O+M|保持 unknown，先证明普通 UI 存在明确表示|
|G030 public descriptions|角色/轻量敌方当前 tooltip 描述链已实现|[P10] [P11] [P12]|A|N部分|SC / 未实机|O+M|补实际覆盖验收；文字不等于完整结构化状态|

## 3. 地形 / 特殊格 / 交互物

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G031 terrain type|Grid 根/尺寸已有，公开 cells=null，格类型仍缺|[P03] [P14]|D|待逆|SC / 未实机|O|复用根与寿命，只补格对象、层和语义|
|G032 passability|当前格通行性未在已审来源闭合|[P03] [P14]|D|待逆|SC / 未实机|O|区分地形、占用、角色能力和方向约束|
|G033 movement cost|角色移动点不等于格代价|[P03] [P14]|D|待逆|SC / 未实机|O|在既有 grid 上补格代价及角色修正|
|G034 LOS interaction|格与对象的遮挡规则仍缺|[P03] [P14]|D|待逆|SC / 未实机|O|定位格/物体遮挡字段和消费者|
|G035 enter effects|CSF 元素反应只提供部分进入效果线索|[C05] [C08]|C|社区|ST / 未实机|M+U|分清进入、接触、移动结束和结算|
|G036 stand effects|元素/affecting-elements 有线索，无完整当前 cell-local 来源|[C05] [C03]|C|社区|ST / 未实机|M|先处理离场 sentinel，不盲调 grid 路径|
|G037 interactables|通用 roster 不是完整交互物清单|[P03] [P14] [C14]|D|待逆|SC / 未实机|O+P|定位交互组件、格绑定和公开状态|
|G038 relative/cell-local rules|技能 kernel 不等于地形格局部规则|[P07] [P14]|D|待逆|SC / 未实机|O|独立记录格约束、邻接影响及覆盖层|

## 4. 地图

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G039 current node|MapMarker→MapScreen→node 已采集公开，不只记最近 EnterNode|[P15] [P13] [P14]|A|N绑定|SC / 未实机|O+M|验冷附加、切换、无当前节点与读取失败|
|G040 node identity|Map/Node incarnation 与不透明 node ID 已接通|[P15] [P17] [P13]|A|N绑定|SC / 未实机|O+M|验同址复用和读档，不用 seed 作公开身份|
|G041 node name|type/destination 可读，但不是公开 name|[P19] [P14]|C|RPM|SC / 未实机|O|从标签/tooltip 取名，不泄露内部 destination|
|G042 node type|kind_raw/type 已有读器，native public 未发|[P19] [P14]|A|RPM|SC / 未实机|O|移植并确认公开图标/类型对应|
|G043 edges / route graph|+0x140/+0x150 两类边向量已有遍历及成员校验|[P19] [P14]|A|RPM|SC / 未实机|O|迁移 raw 图，再补普通 UI 可见边和方向性|
|G044 visited|+0x168 已有 Native→Reader→public 链|[P15] [P14]|A|N绑定|SC / 未实机|O+M|验访问变更、false 与重新生成|
|G045 locked branches|raw flags 可读，剩余字节未证明锁定含义|[P19] [P14]|C|RPM|SC / 未实机|O|追锁定显示及消费者，不猜 flag 命名|
|G046 legal next nodes|边与按钮 callback 不等于下一节点合法性|[P19] [P14]|D|待逆|SC / 未实机|O|取普通 UI 明示资格，不能仅由邻接推断|
|G047 visible metadata|内部 type/destination 已有；当前可见说明仍缺|[P19] [P14]|C|RPM|SC / 未实机|O|补标签/描述控件，不导出未来事件信息|

## 5. 奖励 / 物品 / 装备

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G048 item name|bag/party/Shop 当前 key/name 字符串已有，不全是显示名|[P23] [P21]|A|RPM|SC / 未实机|O|复用 key→资源/当前 tooltip，不重做存储解析|
|G049 item description|有 LevelUp 描述和战斗 tooltip，不代表库存物品完整描述|[P10] [P24] [C11]|C|静态|SC / 未实机|O+G|区分技能 InventoryTooltip 与真正物品 tooltip|
|G050 stats / modifiers|Equipment/CatData 片段可参考，完整有效修正未闭合|[C04] [C12] [P23]|C|社区|ST / 未实机|M+O|保留未知，追当前 modifier 和描述|
|G051 equip slot|read_party 已读五个 0x60 槽的 name/record_id|[P23]|A|RPM|SC / 未实机|O|维护槽布局与显示含义映射|
|G052 restrictions|物品 box/kind/已装备不证明不可卸下等限制|[P22] [P14]|D|待逆|SC / 未实机|O|定位限制显示和消费者，不由按钮存在推断|
|G053 current equipped|当前 party→CatData 五槽及 UI item key 关系已有|[P23] [P22]|A|RPM|SC / 未实机|O|接 Native owner/lifetime；不外推任意敌方装备|
|G054 reward name|LevelUpScreen 当前候选 name/description 已读|[P24]|A|RPM|SC / 未实机|O|适配此类候选，其它奖励屏另证|

## 6. 事件

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G055 title|event_name_key 有读器，最终标题未 Native 采集|[P20]|C|RPM|SC / 未实机|O|沿当前 WorldEvent 补显示文本|
|G056 prompt / body|prompt_key+0xB8 已有，不等于解析后的正文|[P20]|C|RPM|SC / 未实机|O|补当前正文/替换值，不重做根发现|
|G057 options / 容器|Python read_event 已读有界选项数组|[P20]|A|RPM|SC / 未实机|O|迁移 current scene/owner，不调用提交函数|
|G058 options / 文本|option+0x20 UTF-16 label 已读，不再只有 stat key|[P20]|A|RPM|SC / 未实机|O|核对最终渲染和隐藏选项|
|G059 requirements / checks|stat key/按钮名已有，阈值与满足状态未完整解码|[P20]|C|RPM|SC / 未实机|O|关联当前公开检查文本，隐藏条件不披露|
|G060 reward hints|已审路径没有完整公开奖励提示|[P20]|D|待逆|SC / 未实机|O|只取普通 UI 提示，不把未来 outcomes 当提示|
|G061 exposed result type|battle_trigger 仍 unverified，缺当前已公开结果类型|[P20]|D|待逆|SC / 未实机|O|只解析 UI 已明示类型，不执行 choice 探测|

## 7. 商店 / 背包 / 库存

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G062 visible item list|Shop slots/Inventory boxes 已有，实际可见性仍需资格|[P21] [P22]|A|RPM|SC / 未实机|O|移植存储读取，补 Native scene/modal/renderer|
|G063 price|entry+8 price 已读，不再待找地址|[P21]|A|RPM|SC / 未实机|O|核对折扣和货币显示，再接 Native public|
|G064 quantity|entry+12 remaining_stock 已读|[P21]|A|RPM|SC / 未实机|O|区分堆叠/同名实例/特殊无限库存|
|G065 sold-out|已有 stock<=0 诊断实现，不证明本次购买|[P21]|A|RPM|SC / 未实机|O|验证 UI 特殊约定，保留售罄与购买区别|
|G066 currencies|地图 coins 已 Native 公开；food/另类货币仅诊断|[P15] [P21] [P23]|A|N部分|SC / 未实机|O+M|补其它显示与类型，不重做 coins|
|G067 current inventory|read_bag 有只读 linked-map 逐项读取|[P23] [P22]|A|RPM|SC / 未实机|O|优先迁移；补 storage/trash 与跨 UI 寿命|
|G068 current equipment|当前五槽与 bag/UI key 已关联；native public equipment=null|[P23] [P22]|A|RPM|SC / 未实机|O|接新 owner/lifetime，不用 save editor 冒充实时源|

## 8. 升级界面

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G069 current choices|Context.reward 已读 0xF0 options 与按钮 owner/index|[P24] [P25]|A|RPM|SC / 未实机|O|迁移当前选项 source，不重做数组|
|G070 labels|候选宽字符 name 已有，不只是 ID|[P24]|A|RPM|SC / 未实机|O|补最终 label 显示资格|
|G071 descriptions|候选宽字符 description+0x48 已有|[P24]|A|RPM|SC / 未实机|O|复用实际文本，不再列为全新 RE|
|G072 affected unit|LevelUpScreen+0xA0 CatData/cat_id 已读并复核|[P24]|A|RPM|SC / 未实机|O|CatData ID 与战斗 incarnation 分开适配|
|G073 selectable state|committed+0x318、owner/index、部分几何已有|[P24] [P25]|C|RPM|SC / 未实机|O|补 enabled/可见性，index 有效不代表可选|

## 9. Runtime / 生命周期

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G074 loaded/current scene|Director 完成后的 scene 集合、owner、资格入口已有|[P18] [P16] [P13]|A|N绑定|SC / 未实机|O+M|扩新增 screen，不退回只按名称搜索|
|G075 foreground/current scene|Battle/Map 唯一非 paused/parked 候选与变化已处理|[P18] [P14]|A|N绑定|SC / 未实机|O+M|Event/Shop/叠加窗口 admission 分别补证|
|G076 ownership|Character/Brain/Tactics/Grid 和 Map/Node/Button/renderer 已检查|[P03] [P04] [P15]|A|N绑定|SC / 未实机|O+M|复用 owner 原语，为新域补关系|
|G077 active instance|Battle/Map/current actor UI subset 已有绑定|[P18] [P03] [P04]|A|N绑定|SC / 未实机|O+M|补 redirect 和并存 overlay，不整套重建|
|G078 load/reload|原函数前失效、finally 事件计数与 world epoch 已有|[P16] [P17]|A|N绑定|SC / 未实机|O+M|验实际 Hook 配对与覆盖，源码不等于已装 Hook|
|G079 epoch / 局部机制|自建 Scene/Character/Map/Node incarnation；selector 不声称 allocation continuity|[P17] [P05]|A|N绑定|SC / 未实机|O+M|区分身份域，不把 arena stamp 当永不复用 ID|
|G080 epoch / 端到端链|失效 Hook→Director→Reader proof/稳定 ID 已存在|[P16] [P17] [P18] [P13]|A|N绑定|SC / 未实机|O+M|验部署、同址复用、读档、丢 Hook 和跨线程失败|
|G081 camera relationship|toolbar/tooltip compositor 有 camera 资格，另有输入 pose 诊断|[P04] [P10] [P24]|C|N私有|SC / 未实机|O+M|只扩未支持视角/变换；本次未全审 camera 实现|
|G082 input receiver|MewControls/PlayerBrain/CombatMenu 和收件人合同已有|[P04] [P13] [P08]|C|N私有|SC / 未实机|O+M|复用私有 owner；本研究不开展输入实现|
|G083 safe current binding|候选场景、同帧 watch、寿命 proof、freshness 已有有限链|[P16] [P18] [P13] [P04]|A|N绑定|SC / 未实机|O+M|改为剩余边界验证和新域扩展|

## 10. Targeting / Selection

|缺口 / ID|当前状态|最佳现成来源|等级|Runtime 绑定|当前版本验证|许可|下一步|
|---|---|---|:---:|---|---|---|---|
|G084 selected ability/action|selected/action 与 slot/menu 已读；public secondary 仍 unavailable|[P08] [P05] [P14]|A|N私有|SC / 未实机|O+M|补需要的公开投影，不说字段未逆出|
|G085 targeting state|stage/action_type/submitted 及一致性已有|[P08] [P13] [P14]|A|N私有|SC / 未实机|O+M|复用有限状态，补公开语义和 unknown|
|G086 legal target semantics|relative kernel 和坐标命中资格不等于完整合法目标语义|[P07] [P14]|D|待逆|SC / 未实机|O+M+U|研究玩家规则/按需正常高亮，不实现全量策略求解|
|G087 relative targeting|方向、ray、metric、若干 AoE 已有；restricted/custom 未完成|[P06] [P07]|A|N部分|SC / 未实机|O+M|沿已审消费者增补，不重复标准 kernel|
|G088 selection ownership|sample owner、双 callback、selection 重读已有|[P04] [P05] [P08]|A|N私有|SC / 未实机|O+M|补公开 selection 与 redirect/modal 边界|

## 使用结论

优先利用 A/B 中已经存在的有界代码或结构；RPM、Native 私有和 Native 公开状态不能混用。D 表示当前仍缺的语义取证，不表示应重写整个域。所有字段的 unknown 必须区别于 0、false 和已观察的空集合。

[不重复逆向清单](baseline/do-not-reverse-again.md) · [版本与验证模型](evidence/verification-protocol.md) · [生产路径地图](baseline/path-map.md)

[P01]: evidence/production-audit.md#br-entry
[P02]: evidence/production-audit.md#br-sdk
[P03]: evidence/production-audit.md#br-roster
[P04]: evidence/production-audit.md#br-abilities
[P05]: evidence/production-audit.md#br-ability-reader
[P06]: evidence/production-audit.md#br-effective
[P07]: evidence/production-audit.md#br-effective-projection
[P08]: evidence/production-audit.md#br-independent
[P09]: evidence/production-audit.md#br-join
[P10]: evidence/production-audit.md#br-description
[P11]: evidence/production-audit.md#br-description-projection
[P12]: evidence/production-audit.md#br-text-contract
[P13]: evidence/production-audit.md#br-reader
[P14]: evidence/production-audit.md#br-public
[P15]: evidence/production-audit.md#br-map
[P16]: evidence/production-audit.md#br-lifecycle
[P17]: evidence/production-audit.md#br-incarnation
[P18]: evidence/production-audit.md#br-director
[P19]: evidence/production-audit.md#br-rpm-map
[P20]: evidence/production-audit.md#br-rpm-event
[P21]: evidence/production-audit.md#br-rpm-shop
[P22]: evidence/production-audit.md#br-rpm-inventory
[P23]: evidence/production-audit.md#br-rpm-party
[P24]: evidence/production-audit.md#br-rpm-context
[P25]: evidence/production-audit.md#br-rpm-menu
[P26]: evidence/production-audit.md#br-upstream-pins
[P27]: evidence/production-audit.md#br-rule-pins
[P28]: evidence/production-audit.md#br-effective-test
[P29]: evidence/production-audit.md#br-rule-compiler
[P30]: evidence/production-audit.md#br-rule-load
[C01]: evidence/community-sources.md#mewjector
[C02]: evidence/community-sources.md#mewui
[C03]: evidence/community-sources.md#mgmp
[C04]: evidence/community-sources.md#amoeba
[C05]: evidence/community-sources.md#custom-status
[C06]: evidence/community-sources.md#gon
[C07]: evidence/community-sources.md#modding-documentation
[C08]: evidence/community-sources.md#script-resources
[C09]: evidence/community-sources.md#gpak-tiftid
[C10]: evidence/community-sources.md#gpak-shootme
[C11]: evidence/community-sources.md#save-editor
[C12]: evidence/community-sources.md#breeding-helper
[C13]: evidence/community-sources.md#elemental-tooltips
[C14]: evidence/community-sources.md#pilout
