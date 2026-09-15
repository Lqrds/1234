# 不应再次从零逆向的范围

本页用于减少后续重复研究，不授权修改主项目或实机执行。以固定生产基线 `d1edf28` 为准，实际运行验证状态另记。

|已有资产|真正需要补的工作|
|---|---|
|Native 默认 Reader、guarded read / RTTI|不要重新选择注入或加载路线|
|Character/Brain/Tactics/current grid 根及 backlinks|扩展对象语义与 presentation，不重复找根|
|HP/MP/move/attack 原始字段|move/attack 是公开资格缺口，不是 offset 发现|
|当前 ability owner/slot/menu/双 callback|补未支持 UI/redirect 及验收，不写新的槽位系统|
|标准 current cache 范围、距离、相对 AoE 子集|补非零 N/custom/shotgun/restrictions/LOS，不重写已审 kernel|
|完整 open-tooltip 文本与 effect panels|扩展覆盖和关联，不把静态文本或截图当唯一入口|
|Scene/Character/Map/Node 寿命与 Director 完成检查|配对验证和新域覆盖，不整套重设计 epoch|
|Native MapMarker/current/visited/coins|补边、labels、locked 资格，不退回仅记录最近 EnterNode|
|RPM 地图边、商店价格库存、事件 labels|移植现成只读读器，不重新扫内存|
|RPM LevelUp 文字、affected cat、committed|沿现有选项读器迁移，不视为只有社区猜测|
|RPM read_party/read_bag 装备和背包 linked-map|优先只读来源，不主动调用 mgmp 的游戏序列化器|
|GON/GPAK 编译、加载和来源 join|不另造解析器；补需要的字段和实际资源版本|

具体来源和剩余边界见[路径地图](path-map.md)与[主矩阵](../coverage-matrix.md)。**没有本轮实机 readback，不代表源码实现不存在。**
