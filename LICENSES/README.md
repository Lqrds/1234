# 许可登记

本仓库只发布原创审计结论、结构事实和来源定位，不复制主项目实现、第三方实现、SDK、DLL 或游戏资源。根 MIT LICENSE 仅覆盖本仓库原创材料。

主项目基线为用户授权审计的私有仓库 `Lqrds/mewgenics-bridge@d1edf28`。审计时根 LICENSE 未取得，不能据此公开复制其实现；用户将自己的读器迁移到自己的主项目，与向第三方授予复制权是两件事。矩阵中的 A 类并不增加任何许可。

|来源|已审许可状态|复用边界|
|---|---|---|
|Mewjector|MIT|保留原作者版权、许可和实际依赖声明|
|MewUI|MIT|同上；只按确认的代码片段和 ABI 适配|
|mgmp|MIT|同上；研究 pin 与生产 vendor pin 不同|
|Amoeba / mewgenics_analysis|MIT|结构中未知字段继续未知；第三方材料按各自许可|
|CustomStatusFramework|MIT|只在其许可和实际覆盖范围内复用，不能把 custom sidecar 当原版完整枚举|
|TylerGlaiel/GON|MIT|保留 Tyler Glaiel 版权与许可；不授权重分发游戏数据|
|Tiftid GPAK utility|MIT|保留 Tiftid 版权与许可；不授权重分发 GPAK 中资产|
|ShootMe GPAK-Extractor|未确认|保留格式事实和出处，不据此大段复制代码|
|Mewgenics Modding Documentation|未确认|保留文档线索；键字典不等于完整运行时语义|
|MewgenicsScriptResources|未确认|保留字段和相对几何线索，不给未知部分赋确定语义|
|accessiblefish save editor|AGPLv3；only/or-later 未确定|遵守对应条件后再评估代码复用；不把 bundled gamedata 视为自动获授权|
|PurpleMyst breeding helper|MIT|本次 ImHex 子目录版权/许可属于 polymeric，不能只保留根目录署名|
|ElementalTooltipsExpanded|MIT|保留许可；存在高位元素读取缺陷，不宜照搬为完整实现|
|Pilout Mod Framework|未建立开源授权，未取得源码/SDK|保留作者发布线索，取得允许使用的源码/SDK 后再确定搬运方式|

对应固定提交、具体文件和许可文件路径见[社区来源索引](../evidence/community-sources.md)。许可未确认不表示资料没有价值，技术结论和证据应继续保留，但与可直接复制的代码分开。

GON/GPAK 工具许可证、save editor 代码许可证、游戏资源版权相互独立。本仓库不重新发布游戏文本、整套定义表、美术或资源包。
