---
name: tModLoader 代码注释规范
description: 用于为 tModLoader / Terraria 模组代码补充清晰注释，尤其在用户无法查看原版游戏源码时——要求说明每个钩子/字段的含义与调用时机、标注涉及的原版 ID，并附上对应类/钩子的 WIKI 或 API 文档链接。Use when writing or improving comments/annotations in tModLoader mod code.
---

# tModLoader 代码注释规范

本技能规范模组代码的**注释写法**，服务于一个前提：**用户通常无法查看 Terraria / tModLoader 的原版源码**。因此注释不只是"复述代码"，而要替读者解释"这段 API 是什么、什么时候被调用、原版默认行为是什么、为什么这样写"。

> 先遵循主技能 `tmod-modding` 的「先说明 → 等确认 → 再执行」铁律。本技能只补充"注释怎么写"。

## 1. 何时使用

- 用户要求"注释写清楚一点 / 加上说明 / 看不懂这段"。
- 新增或改写内容类之后（尤其是 `Global*` 改写原版、`ModProjectile` / `ModNPC` 等含 AI 与状态机的代码）。
- 代码里出现原版 ID、魔法数字、贴图朝向、无敌帧、网络同步等"不看原版源码就难懂"的地方。

## 2. 核心原则

1. **只加/改注释，不动逻辑。** 不改任何语句、数值、命名。
2. **面向看不到源码的读者**：解释 `What / When / Why`，而不是把代码翻译一遍。
3. **每个 override 回答三问**：何时被调用？默认行为是什么？这里为什么重写？
4. **常量与魔法数字**：注明含义、单位（帧=1/60 秒、像素、比例）。
5. **附可点击链接**：指向官方 WIKI 或 API 文档，且链接必须已验证可访问。
6. **语言**：说明文字用中文；类型名、方法名、原版 ID 保持英文原样。

## 3. 三层注释结构

**① 文件头块注释**（在 `namespace` 之后、类声明之前，或类内首行）：

```text
// ============================================================================
// <类名> —— <一句话作用>
// ============================================================================
// 【这是什么】该类属于哪类内容、对应哪只/哪件原版目标。
// 【为什么这么做】为何用 Global* 改写原版 / 为何新建内容类。
// 【机制】整体流程或状态机（用 A -> B -> C 表示）。
// 【涉及的原版 ID】ItemID./ProjectileID./BuffID./NPCID. 等，并给出 ID 查询链接。
// 【相关文档】类文档与基础教程链接。
// ============================================================================
```

**② 每个 override / 公开方法上方**：说明调用时机、参数含义、与原版的关系、本处意图。涉及特殊平台行为（如贴图朝向、同步）时给出链接。

**③ 常量与关键行**：行内注释注明单位与作用；易踩坑处（伤害被清零、`spriteDirection` 翻转、`alpha` 淡入等）单独说明。

## 4. 常见钩子/类型的语义速查（写注释时可直接引用）

| 类型 / 成员 | 何时调用 / 含义 | 注释要点 |
| --- | --- | --- |
| `GlobalProjectile` / `GlobalItem` / `GlobalBuff` | 挂到"所有"对应对象上（含原版） | 说明用 `AppliesToEntity` 缩小到目标；类文档链接 |
| `InstancePerEntity` | 决定 Global 实例是否每个对象一份 | 需要"每只/每件单独存状态"时必须为 true |
| `SetStaticDefaults` | 注册阶段调用一次，静态数据 | 不能访问具体实例 |
| `SetDefaults` | 实例初始化末尾 | 覆盖原版默认属性 |
| `PostAI` / `PreAI` | 原版 AI 每帧前后 | `PreAI` 返回 false 会拦截原版 AI；`PostAI` 总是执行 |
| `MinionContactDamage` | 宠物/随从接触伤害开关 | 需显式返回 true 才会撞伤敌人 |
| `OnHitNPC` | 命中敌人时 | 参数 `NPC.HitInfo` 携带本次伤害/暴击/击退 |
| `ModifyTooltips` / `ModifyBuffText` | 构造提示文本时 | 参数 `ref` 可原地替换；本地化文本放在 hjson |
| `SendExtraAI` / `ReceiveExtraAI` | 多人同步自定义数据 | 与 `netUpdate = true` 配套；读写顺序须一致 |
| `ModProjectile.ai[]` | 弹幕自定义数值槽（随弹幕同步） | 说明每个下标含义 |
| `Projectile.NewProjectile` | 生成弹幕 | 需带 `IEntitySource`（如 `GetSource_FromThis/FromAI`） |
| `NPC.CanBeChasedBy` | 索敌过滤 | 排除友方/无敌目标 |
| `texture` 覆盖 | 复用原版贴图 | `"Terraria/Images/Projectile_<ID>"` 约定路径 |

## 5. 链接规范与映射表

- 统一格式：`// 参考: <url>`（放在相关方法/段落上方或行尾）。
- **不得编造链接**。写前用 HTTP 校验（见 §7）。注意：`GlobalProjectile` 等类型**没有独立 WIKI 页**，应链接到官方 API 文档而非猜 wiki 页名。

以下为**已验证可访问**的常用链接：

**API 类文档（官方 docs.tmodloader.net，1.4-stable）**

- GlobalProjectile: `https://docs.tmodloader.net/docs/1.4-stable/class_terraria_1_1_mod_loader_1_1_global_projectile.html`
- GlobalItem: `https://docs.tmodloader.net/docs/1.4-stable/class_terraria_1_1_mod_loader_1_1_global_item.html`
- GlobalBuff: `https://docs.tmodloader.net/docs/1.4-stable/class_terraria_1_1_mod_loader_1_1_global_buff.html`
- ModProjectile: `https://docs.tmodloader.net/docs/1.4-stable/class_terraria_1_1_mod_loader_1_1_mod_projectile.html`

**WIKI 指南（github.com/tModLoader/tModLoader/wiki）**

- 基础弹幕 `Basic-Projectile`
- 基础物品 `Basic-Item`
- 随从/宠物 `Basic-Minion-Guide`
- 本地化 `Localization`
- 原版 ID 总表 `Vanilla-Content-IDs`
- 原版弹幕 AI `Vanilla-Projectile-AIs`
- 原版弹幕字段 `Vanilla-Projectile-Field-Values`
- 弹幕类文档 `Projectile-Class-Documentation`
- 物品类文档 `Item-Class-Documentation`
- 常用原版字段/方法 `Useful-Vanilla-Fields`、`Useful-Vanilla-Methods`
- 网络同步 `Basic-Netcode`
- 生成来源 `IEntitySource`

（WIKI 链接前缀统一为 `https://github.com/tModLoader/tModLoader/wiki/`。）

## 6. 方法与行内模板

```csharp
// AppliesToEntity：<何时调用>。这里只认 <原版 ID>，其它对象不受影响。
// 参考: <类文档链接>
public override bool AppliesToEntity(...) { ... }

// <钩子名>：<何时调用 / 默认行为 / 这里为何重写>。
// 参考: <相关链接>
public override void SomeHook(...) { ... }

private const int Cooldown = 300; // 间隔（帧），300 = 5 秒
```

## 7. 自检清单（完成后逐条确认）

- [ ] 除注释外没有任何代码改动（可对照改动前后的非注释行）。
- [ ] 每个 override 都回答了"何时调用 / 默认行为 / 为何重写"。
- [ ] 常量与魔法数字说明了单位与含义。
- [ ] 涉及的原版 ID 已标注，并给出 ID 查询链接。
- [ ] 所有新增链接均已验证返回 200（示例：`Invoke-WebRequest -Uri <url> -Method Get`）。
- [ ] 文件无重复代码块、无混入的非代码文本。
- [ ] 构建通过（如 `dotnet build`）。构建失败或环境缺失时如实说明，不谎报通过。

## 8. 禁止事项

- ❌ 借"加注释"之名修改逻辑、数值或命名。
- ❌ 编造 API 名称、参数或链接（拿不准就查证或标注"未验证"）。
- ❌ 用"显而易见""顾名思义"等无信息量的话。
- ❌ 只复述代码表面行为，不解释钩子语义与调用时机。
- ❌ 省略"坑点"（如贴图朝向、伤害被清零、同步遗漏），这些恰恰是看不到源码时最需要的。
