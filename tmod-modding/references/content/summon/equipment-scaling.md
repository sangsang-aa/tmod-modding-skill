# 饰品 / 药水 / 召唤栏位加成

> 上级：`index.md`。本文件只讲「如何让召唤伤害正确吃到装备与药水加成」。

## 1. 铁律

- 物品与弹幕都必须设 `DamageType = DamageClass.Summon`（鞭子用 `SummonMeleeSpeed`）。
- **不要硬编码最终伤害 / 击退**，否则召唤伤害饰品、伤害药水、增益全部白费。
- 召唤栏位不要写死，否则召唤药水、施法台、相关饰品不生效。

## 2. 自定义射击：按 owner 结算

```csharp
private static void Shoot(Projectile projectile, int type, int baseDamage, float baseKnockback, Vector2 velocity)
{
    Player owner = Main.player[projectile.owner];

    // 汇总：召唤伤害饰品、伤害药水、增益、前缀等全部包含在内
    int damage = (int)owner.GetTotalDamage(DamageClass.Summon).ApplyTo(baseDamage);
    float knockback = owner.GetKnockback(DamageClass.Summon).ApplyTo(baseKnockback);

    Projectile.NewProjectile(projectile.GetSource_FromAI(), projectile.Center, velocity, type, damage, knockback, projectile.owner);
}
```

## 3. 随从 / 哨兵本体

- 随从与哨兵的 `Projectile.damage` 会**自动随当前装备更新**（`ContinuouslyUpdateDamageStats`），不要在 AI 里反复重算。
- 召唤栏位使用：

```csharp
// 不要写 const int MaxMinions = 3;
player.maxMinions;                                 // 已汇总召唤药水/施法台/饰品
Projectile.minionSlots = 1f;                       // 该随从占用的栏位
ItemID.Sets.StaffMinionSlotsRequired[Type] = 1f;   // 法杖召唤时占用的栏位
```

## 4. 鞭子的攻速 / 范围

- 鞭子的攻速、范围、段数由 vanilla 鞭子设置处理：
  - 物品：`Item.DefaultToWhip(...)`
  - 弹幕：`Projectile.DefaultToWhip()`、`Projectile.WhipSettings`
- 不要手写速度 / 范围来「模拟」鞭子，否则相关饰品与药水修正会被绕过。

## 5. 这些加成分别来自哪里

| 来源 | 如何生效 |
| --- | --- |
| 召唤伤害饰品（如召唤师徽章、死灵卷轴等） | 汇总进 `GetTotalDamage(DamageClass.Summon)` |
| 通用伤害药水 / 增益 | 汇总进 `GetTotalDamage` |
| 召唤药水 / 施法台 / 增加栏位的饰品 | 体现在 `player.maxMinions` |
| 召唤击退饰品 | 汇总进 `GetKnockback(DamageClass.Summon)` |
| 鞭子攻速 / 范围饰品 | 由 vanilla 鞭子逻辑与 `WhipSettings` 处理 |

> 不要逐个判断「玩家有没有某个饰品」来手动加数值；统一走上述汇总接口，才能兼容原版与其它模组。

## 6. 自检表

| 检查项 | 不合格的表现 |
| --- | --- |
| 伤害走 `GetTotalDamage` | 换饰品、喝药水后伤害不变 |
| 击退走 `GetKnockback` | 击退不受召唤击退饰品影响 |
| `DamageType = DamageClass.Summon` | 伤害变成真伤，全部加成失效 |
| 召唤栏位用 `maxMinions` / `minionSlots` | 召唤药水、施法台、饰品无效 |
| 鞭子用 `DefaultToWhip` / `WhipSettings` | 鞭子攻速/范围不吃饰品修正 |
| 自定义射击在 owner 端生成 | 多人游戏伤害/表现不同步 |
