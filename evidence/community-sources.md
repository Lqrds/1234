# 社区来源及固定版本

本页对应矩阵 C01–C14。沿用前序代码级审计的固定提交，不声称本次重新完整审计所有最新 HEAD。许可与验证只覆盖标明的文件/片段；所有来源均未在本轮目标运行环境完成实机验证。README 声称不按实现接受。

## mewjector

C01 · [`githubuser508/mewjector@ccdd6813cef0f51342eb74c0cecb47654f7dbeef`](https://github.com/githubuser508/mewjector/tree/ccdd6813cef0f51342eb74c0cecb47654f7dbeef)。MIT，许可文件 `LICENSE`。

已查 `mewjector.h`、`version.c` 部分加载路径。`MewjectorAPI`、`MJ_ResolveVersion`、`InstallHook/QueryHook/AllocTypeIdPair/RegisterName/GetGameBase/VerifyHooks` 是加载与 Hook 协调，不提供当前单位、场景、成本或合法目标。type ID 不是对象 epoch。限定 API 为 `source_confirmed`；完整 Hook 实现没有获得全面认证。

## mewui

C02 · [`Pseudonym-Tim/mewgenics-ui-api@fffef60696c50f0748052da8e6a1fb12dfaabbe9`](https://github.com/Pseudonym-Tim/mewgenics-ui-api/tree/fffef60696c50f0748052da8e6a1fb12dfaabbe9)。MIT，`LICENSE`。

定位 `src/native/mew_ui_api.h/.c`、`re_tools/mew_ui_api_signatures.json`。MewNarrowString/MewWideString、指针容器、组件关系、Button label/state 与 loaded-scene 跟踪可复用。其本地 scene generation 不是所有游戏对象的创建/销毁证明；首个同名 loaded scene 不等于唯一前台 scene。相关片段 `source_confirmed/structure_confirmed`，不是全仓库审计。

## mgmp

C03 · [`SanTertrust/mgmp@bcd9985bcf7b388df1dd03006cd66fc944d91015`](https://github.com/SanTertrust/mgmp/tree/bcd9985bcf7b388df1dd03006cd66fc944d91015)。MIT，`LICENSE`。

重点文件：`src/determinism/mgmp_ability.cpp`（`ability_slot_of`、`ability_from_slot`、`ability_gon_name`），`src/session/mgmp_lockstep.cpp`（`read_cat_state`、roster membership），`mgmp_choice.cpp`、`mgmp_follow.cpp`、`mgmp_aim.cpp/.h`、`mgmp_invsync.cpp/.h`。

槽位/GON 身份、HP/shield、候选与节点容器、Inventory 根是有价值的 `source_confirmed` 实现。只提取限定 reader/结构，不移植联机重放、输入或写入路径。当前 UI selection 与 committed decision 不同。范围 preview 可写状态甚至临时移动角色；Inventory 同步主动调用原生序列化，不能当纯只读查询。

**生产 vendor pin 是 `16cc79cfa48f41cef2e9afdcd1f72550a18db09b`，不是本研究的 bcd9985。** 具体生产已吸收什么，以 [BR-UPSTREAM-PINS](production-audit.md#br-upstream-pins) 与调用点为准。

## amoeba

C04 · [`p0lymeric/mewgenics_analysis@440690ce380276035b4abba790482361427d482c`](https://github.com/p0lymeric/mewgenics_analysis/tree/440690ce380276035b4abba790482361427d482c)。MIT，`LICENSE.md`。

定位 `cpp/amoeba/types/glaiel_ecs.hpp`、`glaiel_cat.hpp`、`glaiel_toplevel.hpp`、`gon.hpp`。有 Component/Entity/Scene、VirtualGenerationalArenaAllocator/VGAAElement、CatData、CampaignStats、Equipment、MewDirector 的有限结构。布局中的未知部分保持未知；CatData 是持久猫数据，不是敌方当前 Character。GonObject 的 native layout 也不保证与离线 parser ABI 相同。相关结构 `structure_confirmed`，没有全程序运行验证。

## custom-status

C05 · [`githubuser508/CustomStatusFramework@e3462004c6fa98e19f74a525770a7b63ed1e6301`](https://github.com/githubuser508/CustomStatusFramework/tree/e3462004c6fa98e19f74a525770a7b63ed1e6301)。MIT，`LICENSE`。

定位 `csf_core.h`、`csf_core_api.h`、`csf_core.c` 的 `CSFCore_GetSidecarForInstance` 导出，以及 donor/slot 文件。Passive bearer、counter、registry blob 与 `CSFSidecarView/CSFStatusConfigView` 有明确线索和定义。custom sidecar 只覆盖框架创建的自定义状态，不是全部原版状态枚举；计数可能表示不同状态的层数、持续或效果量。

本次此前审计为头文件选定范围、导出存在性与结构片段；没有全审 `csf_core.c` 或全部 donor。部分元素位语义仍推测，并与另一项目存在命名冲突，不能强行合并。最高仅相应的 `source_confirmed/structure_confirmed`。

## gon

C06 · [`TylerGlaiel/GON@7f9600b278231a1d458e0ca7f44784e10cffa953`](https://github.com/TylerGlaiel/GON/tree/7f9600b278231a1d458e0ca7f44784e10cffa953)。MIT，`LICENSE`。

已查 `gon.h` 全文和 `gon.cpp` 1–190 行。`GonObject`、Load/LoadFromBuffer、children_array/children_map、DeepMerge/PatchMerge 接口提供解析与合并基础。重复键仍保存在有序 children_array，不能简单压成丢失信息的普通字典。完整 merge 实现未在社区审计中全部复核；生产 compiler 的实际调用另见 [BR-RULE-COMPILER](production-audit.md#br-rule-compiler)。离线语法已知不等于游戏规则求值已知。

## modding-documentation

C07 · [`Jzurcc/Mewgenics-Modding-Documentation@25701798f918c980246dd599ffb1156c72ce6a1c`](https://github.com/Jzurcc/Mewgenics-Modding-Documentation/tree/25701798f918c980246dd599ffb1156c72ce6a1c)。许可未确认。

已查 `Schema/Core_Entities_and_Combat/Abilities_and_Spells.md` 节选和 Schema 索引。它是自动生成键字典/资源目录；字段出现、推断类型和样例不证明全部引擎执行语义。其它生成域未完整审核。仅 `documented`，保留线索，不大段复制源文档或游戏表。

## script-resources

C08 · [`ombrellus/MewgenicsScriptResources@f5b9c9231722013715a54750168acde40ae76d9c`](https://github.com/ombrellus/MewgenicsScriptResources/tree/f5b9c9231722013715a54750168acde40ae76d9c)。许可未确认。

已查 `ability_fields.md` 1–410 行，涉及 meta/name/desc/tooltip_values、cost、target_mode/range_mode、custom_range/custom_aoe、symmetry、restrictions 等。它明确仍不完整；坐标解释、LOS、距离模式与特殊规则未逐项实机确认。为 `documented` 线索，不是可直接当 Runtime 求值器的实现。

## gpak-tiftid

C09 · [`Tiftid/mewgenics_gpak_util@313e4f5f719eba6e513c0be27677908580175fdb`](https://github.com/Tiftid/mewgenics_gpak_util/tree/313e4f5f719eba6e513c0be27677908580175fdb)。MIT，`LICENSE`。

已查 `src/main.zig` 1–220 行。`Dictionary.read`、`Dictionary.Entry.read` 定义文件数和逐条路径/长度目录读取；首四字节是文件计数，不是 magic。属 `source_confirmed` 格式实现，未全面审其所有提取/写出安全性。生产已有受限 list/exact-extract 适配；无需重新实现容器。

## gpak-shootme

C10 · [`ShootMe/GPAK-Extractor@d9687b029e408dd3e001440d0478b8c1f4f29342`](https://github.com/ShootMe/GPAK-Extractor/tree/d9687b029e408dd3e001440d0478b8c1f4f29342)。许可未确认。

`Program.cs` 的 GPAK.ReadFile、Entry.Read 提供独立格式交叉证据。对短读、路径与输出边界不能按完整安全工具照搬；只保留 `source_confirmed` 格式结论及出处，不复制实现。

## save-editor

C11 · [`accessiblefish/mewgenics-save-editor@a3fc14fb16a019bdcaba47998958960e6fae641b`](https://github.com/accessiblefish/mewgenics-save-editor/tree/a3fc14fb16a019bdcaba47998958960e6fae641b)。`LICENSE` 为 AGPLv3，only/or-later 未确定。

已查 `mewgenicseditor/parser.py` 1–240 行与 `gamedata.py` 全文。LZ4/存档包装、`load_localized_csv`、`localized_text`、有限 GON block 提取是有用实现，但不是当前实例源或完整 GON parser。没有全审装备解析器；bundled gamedata 的资源版权不随代码许可自动开放。

## breeding-helper

C12 · [`PurpleMyst/mewgenics_breeding_helper@2b461b288156e122f838fbc75c763c0e8028a289`](https://github.com/PurpleMyst/mewgenics_breeding_helper/tree/2b461b288156e122f838fbc75c763c0e8028a289)。MIT；本次 `imhex_patterns/LICENSE.md` 属于 polymeric，根目录另有 PurpleMyst 署名。

`imhex_patterns/cat.hexpat` 有序列化 CatData/Equipment 定义、五槽、格式版本与目标 build 说明。序列化顺序不是 RAM offset；未知字段和较早版本处理继续未解决。它对格式片段的 `structure_confirmed` 不证明当前游戏内猫/装备绑定。

## elemental-tooltips

C13 · [`githubuser508/elementaltooltipsexpanded@1c73b54f9262611433b33b21a816af3f0aafa3af`](https://github.com/githubuser508/elementaltooltipsexpanded/tree/1c73b54f9262611433b33b21a816af3f0aafa3af)。MIT，`LICENSE`。

已查 `ElementalTooltipsExpanded.h` 全文和 `.c` 1–240 行。Ability 元素字段、图标装配 hook 和 SWF 子节点查找可作线索。`OverrideElementIcons` 把三个 uint32 扩展到 uint64 后 OR，不能生成高 32 位，因此不能支持其表中 32–37 位的声称范围。不能把其“所有元素”宣传作为已完整实现；未实机测试。

## pilout

C14 · [Pilout / Mewgenics Mod Framework 发布页](https://www.nexusmods.com/mewgenics/mods/183)。**未取得可审源码或 SDK，未建立开源授权；无可固定的源码 commit。**

作者发布资料里的 Fight/terrain/object 能力是值得保留的后续线索，但此处仅 `documented`，不接受 README/发布说明作为已完成源码逆向。若取得作者允许使用的源码/SDK，应重新检查具体 API、结构与当前 Runtime 绑定，再提升等级。

## 与生产的关系

上述社区成果不等于全部已装入生产。优先检查主项目实际 vendor pin、被调用的函数和 Native/RPM/公开投影层，再确定真正剩余工作。[生产证据](production-audit.md) · [主矩阵](../coverage-matrix.md) · [许可](../LICENSES/README.md)
