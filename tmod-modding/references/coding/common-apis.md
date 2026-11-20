# 常用 API 与钩子速查

> 只列高频项。不确定时以 `ExampleMod` 与 <https://docs.tmodloader.net/docs/1.4-stable/> 为准。

## 1. 常用常量

| 用途 | 常量 |
| --- | --- |
| 伤害类型 | `DamageClass.Melee/Ranged/Magic/Summon/Throwing/SummonMeleeSpeed` |
| 物品稀有度 | `ItemRarityID.*` |
| 使用方式 | `ItemUseStyleID.*` |
| 音效 | `SoundID.*` |
| 原版 ID | `ItemID.*`、`ProjectileID.*`、`NPCID.*`、`TileID.*`、`BuffID.*`、`WallID.*` |
| NPC AI | `NPCAIStyleID.*` |

原版 ID 查询：<https://github.com/tModLoader/tModLoader/wiki/Vanilla-Content-IDs>

## 2. 常用辅助方法

- `Item.DefaultToMagicWeapon(...)` / `DefaultToRangedWeapon(...)` / `DefaultToThrowingWeapon(...)`
- `Item.DefaultToAccessory(...)` / `DefaultToPlaceableTile(...)` / `DefaultToWhip(...)`
- `Item.CloneDefaults(ItemID.…)`
- `Projectile.DefaultToWhip()`
- `Item.buyPrice(...)` / `Item.sellPrice(...)`

## 3. 全局类钩子

| 钩子 | 用途 |
| --- | --- |
| `GlobalItem.AppliesToEntity` | 缩小到某件原版物品 |
| `GlobalItem.ModifyTooltips` | 改提示文本 |
| `GlobalBuff.ModifyBuffText` | 改增益说明 |
| `GlobalProjectile.AppliesToEntity` | 缩小到某个原版弹幕 |
| `GlobalProjectile.PostAI` | 在原版 AI 之后追加逻辑 |
| `GlobalProjectile.OnHitNPC` | 命中回调 |
| `GlobalNPC.ModifyHitByProjectile` | 改命中结算（鞭子 tag 加成） |
| `GlobalNPC.OnKill` | 追加掉落等 |

- `InstancePerEntity => true`：需要每个实体单独存状态时使用（如每只宠物一个计时器）。

## 4. 网络同步

- 改了自定义字段后设置 `projectile.netUpdate = true` 触发同步。
- 自定义字段用 `SendExtraAI` / `ReceiveExtraAI` 发送与接收，读写顺序必须一致。
- 详见 <https://github.com/tModLoader/tModLoader/wiki/Basic-Netcode>

## 5. 生成来源

- `Projectile.NewProjectile(IEntitySource source, ...)` 必须带来源：
  - 物品：`Item.GetSource_FromThis()`
  - 弹幕 AI：`projectile.GetSource_FromAI()`
  - 玩家：`player.GetSource_*`
- 详见 <https://github.com/tModLoader/tModLoader/wiki/IEntitySource>

## 6. 索敌

- `NPC.CanBeChasedBy()`：过滤友方/无敌等不可攻击目标。
- `Collision.CanHitLine(...)`：视线检查。
- 召唤类优先索敌见 `../content/summon/whip-targeting.md`。
