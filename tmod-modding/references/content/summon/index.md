# 召唤类强制清单（总则）

> 适用范围：随从、哨兵、召唤武器、鞭子，以及**把原版宠物改写成会攻击**的内容。
> 本文件是召唤类的入口；三个技术细项在下方子文档中展开，**全部为强制项，不得省略**。

## 1. 三条强制逻辑

### 1.1 鞭子的优先索敌

- 索敌顺序固定：`Projectile.OwnerMinionAttackTargetNPC`（鞭子命中 / 右键标记）**优先**，其次才是最近的 `CanBeChasedBy()` 敌人。
- 自定义鞭子命中时必须写入标记：`Main.player[Projectile.owner].MinionAttackTargetNPC = target.whoAmI;`
- 随从 / 哨兵需登记 `ProjectileID.Sets.MinionTargettingFeature[Type] = true;`，右键标记才生效。
- 详见 → `whip-targeting.md`

### 1.2 鞭子的加成

- 随从本体：`Projectile.minion = true;`、`Projectile.minionSlots = 1f;`、`ProjectileID.Sets.MinionSacrificable[Type] = true;`
- 随从 / 哨兵射击：`MinionShot[Type] = true;` / `SentryShot[Type] = true;`
- 鞭子 tag 增益：`BuffID.Sets.IsATagBuff[Type] = true;`，并在 `GlobalNPC.ModifyHitByProjectile` 中仅对 `projectile.IsMinionOrSentryRelated` 生效，按 `ProjectileID.Sets.SummonTagDamageMultiplier[projectile.type]` 缩放。
- 详见 → `whip-tag-bonuses.md`

### 1.3 饰品 / 药水加成不可绕过

- **禁止硬编码最终伤害 / 击退**。
- 自定义射击必须：
  - `owner.GetTotalDamage(DamageClass.Summon).ApplyTo(baseDamage)`
  - `owner.GetKnockback(DamageClass.Summon).ApplyTo(baseKnockback)`
- 物品与弹幕都必须设 `DamageType = DamageClass.Summon`。
- 召唤栏位不要写死：用 `player.maxMinions` / `Projectile.minionSlots` / `ItemID.Sets.StaffMinionSlotsRequired`。
- 详见 → `equipment-scaling.md`

## 2. 通用自检

- [ ] 标记目标优先于最近目标。
- [ ] 随从 / 哨兵 / 射击已登记对应 `Minion` / `MinionShot` / `Sentry` / `SentryShot`。
- [ ] 鞭子命中已写入 `MinionAttackTargetNPC` 并施加 tag 增益。
- [ ] 伤害 / 击退经过 `GetTotalDamage` / `GetKnockback`，没有写死数值。
- [ ] 召唤栏位使用 `maxMinions` / `minionSlots`，没有硬编码。
- [ ] `DamageType = DamageClass.Summon` 已设置。
- [ ] 鞭子攻速 / 范围走 `DefaultToWhip` / `WhipSettings`，没有手写绕过。

## 3. 参考

- `ExampleMod/Content/Buffs/ExampleWhipDebuff.cs`
- `ExampleMod/Content/Items/Weapons/ExampleWhip.cs`
- `ExampleMod/Content/Projectiles/ExampleWhipProjectile.cs`
- `ExampleMod/Content/Projectiles/Minions/ExampleSimpleMinion.cs`
- `ExampleMod/Content/Projectiles/Minions/ExampleSentry.cs`
- 随从指南：<https://github.com/tModLoader/tModLoader/wiki/Basic-Minion-Guide>
- 原版 ID：<https://github.com/tModLoader/tModLoader/wiki/Vanilla-Content-IDs>
