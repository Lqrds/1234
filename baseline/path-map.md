# 新生产路径地图

固定基线：`d1edf28d7bd09dab7e051763aaf642f33a650a75`。这是实际代码职责路线，不是声称所有文件都发生了重命名。下表 `src/` 简写以主项目 `packages/native-backend/` 为根。

|职责|当前入口 / 链路|证据|边界|
|---|---|---|---|
|默认入口|`packages/sdk/index.mjs → packages/runtime-win32/index.mjs → packages/native-backend/reader.mjs`|[BR-ENTRY](../evidence/production-audit.md#br-entry)、[BR-SDK](../evidence/production-audit.md#br-sdk)、[BR-READER](../evidence/production-audit.md#br-reader)|`runtime-win32` 目录名不表示默认使用 Python；无静默 RPM fallback|
|Battle 公开数据|`src/battle_roster.inc → reader.mjs → packages/core/player-view.mjs`|[BR-ROSTER](../evidence/production-audit.md#br-roster)、[BR-PUBLIC](../evidence/production-audit.md#br-public)|公开仅 presented/HUD 合格子集；原始计数已读，公开可能仍为 null|
|技能与显示|`src/current_abilities.inc → abilities.mjs → core/ability-rules.mjs → player-view.mjs`|[BR-ABILITIES](../evidence/production-audit.md#br-abilities)、[BR-ABILITY-READER](../evidence/production-audit.md#br-ability-reader)、[BR-JOIN](../evidence/production-audit.md#br-join)|slot 0=move、1=attack、2+=active；当前 UI subset 不是全技能清单|
|当前有效规则|`src/current_effective.inc / src/independent_rules.inc → effective-rules.mjs → core/ability-rules.mjs`|[BR-EFFECTIVE](../evidence/production-audit.md#br-effective)、[BR-INDEPENDENT](../evidence/production-audit.md#br-independent)、[BR-EFFECTIVE-PROJECTION](../evidence/production-audit.md#br-effective-projection)|v2 允许已独立证明的当前值不同于静态；相对 kernel 不等于合法目标集合|
|完整 tooltip|`src/current_descriptions.inc → descriptions.mjs → core/tooltip-description.mjs → player-view.mjs`|[BR-DESCRIPTION](../evidence/production-audit.md#br-description)、[BR-DESCRIPTION-PROJECTION](../evidence/production-audit.md#br-description-projection)、[BR-TEXT-CONTRACT](../evidence/production-audit.md#br-text-contract)|完整新 DESCRIPTION_SNAPSHOT 替代旧帧，不把新文本拼入旧帧；当前已打开 tooltip 才有资格|
|Map/current/visited/coins|`src/map_observation.inc → reader.mjs → player-view.mjs`|[BR-MAP](../evidence/production-audit.md#br-map)|图边与类型的诊断读取不表示完整 native 公开图已完成|
|生命周期|`src/lifecycle.inc + src/lifetime_registry.inc + src/director_completion.inc → reader.mjs`|[BR-LIFECYCLE](../evidence/production-audit.md#br-lifecycle)、[BR-INCARNATION](../evidence/production-audit.md#br-incarnation)、[BR-DIRECTOR](../evidence/production-audit.md#br-director)|限定 Scene/Character/Map/Node；实际 Hook 安装与完整覆盖本轮未验证|
|事件|`packages/runtime-win32/profiles/current/event.py`|[BR-RPM-EVENT](../evidence/production-audit.md#br-rpm-event)|已有 option label；title/prompt 仍只是键|
|商店|`packages/runtime-win32/profiles/current/shop.py`|[BR-RPM-SHOP](../evidence/production-audit.md#br-rpm-shop)|price/stock/item keys 现成，但默认 Native 不因此支持 Shop scene|
|升级 / 奖励|`packages/runtime-win32/profiles/current/context.py::reward + menu.py`|[BR-RPM-CONTEXT](../evidence/production-audit.md#br-rpm-context)、[BR-RPM-MENU](../evidence/production-audit.md#br-rpm-menu)|对应 LevelUpScreen，不涵盖所有 reward 交互|
|背包 / 装备|`packages/runtime-win32/profiles/current/native.py::read_party/read_bag + inventory.py`|[BR-RPM-PARTY](../evidence/production-audit.md#br-rpm-party)、[BR-RPM-INVENTORY](../evidence/production-audit.md#br-rpm-inventory)|优先现成只读 linked-map，不必主动调用引擎序列化器|
|地图图边|`packages/runtime-win32/profiles/current/map.py`|[BR-RPM-MAP](../evidence/production-audit.md#br-rpm-map)|两类边容器已有；公开可见、锁定与可选语义仍另证|
|静态资源|`packages/rules-source/native/gon_rule_catalog.cpp → packages/rules-source/index.mjs`|[BR-RULE-COMPILER](../evidence/production-audit.md#br-rule-compiler)、[BR-RULE-LOAD](../evidence/production-audit.md#br-rule-load)|显式目录文件的编译/加载，不是所有启动默认都有完整 catalog|
|上游版本|`packages/native-backend/provenance.json / packages/rules-source/provenance.json`|[BR-UPSTREAM-PINS](../evidence/production-audit.md#br-upstream-pins)、[BR-RULE-PINS](../evidence/production-audit.md#br-rule-pins)|mgmp 研究 pin 与生产 vendor pin 不同；不把整个 mgmp 当成已接入|

Native 事实快照只接受 Battle/Map 的判断来自 Reader validator，而非 README 或文件名。Python RPM 读取的是 C++ 对象，也仍是外部诊断后端。

[返回主矩阵](../coverage-matrix.md)
