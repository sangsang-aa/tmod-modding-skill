# 鞭子的加成与 tag 结算

> 上级：`index.md`。本文件只讲「鞭子武器、tag 增益、以及随从/哨兵如何被识别为可吃加成」。

## 1. 鞭子物品

```csharp
public override void SetDefaults()
{
    // 一次性设好伤害类型(SummonMeleeSpeed)、射弹、使用方式等
    Item.DefaultToWhip(ModContent.ProjectileType<MyWhipProjectile>(), 20, 2, 4);
    Item.rare = ItemRarityID.Green;
}
```

## 2. 鞭子弹幕

```csharp
public override void SetStaticDefaults()
{
    // 使用鞭子的命中判定，并允许药剂(flask)效果
    ProjectileID.Sets.IsAWhip[Type] = true;
}

public override void SetDefaults()
{
    Projectile.DefaultToWhip();
    // 可选：Projectile.WhipSettings.Segments = 20;
    // 可选：Projectile.WhipSettings.RangeMultiplier = 1f;
}
```

## 3. 鞭子 tag 增益

```csharp
public class MyWhipDebuff : ModBuff
{
    public static readonly int TagDamage = 5;

    public override void SetStaticDefaults()
    {
        // 允许施加到“免疫所有减益”的敌人身上，并让其它系统识别它是 tag 增益
        BuffID.Sets.IsATagBuff[Type] = true;
    }
}
```

## 4. 在 GlobalNPC 中结算 tag 加成

```csharp
public class MyWhipDebuffNPC : GlobalNPC
{
    public override void ModifyHitByProjectile(NPC npc, Projectile projectile, ref NPC.HitModifiers modifiers)
    {
        // 只让玩家的随从/哨兵相关攻击吃到加成
        if (projectile.npcProj || projectile.trap || !projectile.IsMinionOrSentryRelated) {
            return;
        }

        // 某些弹幕的 tag 收益需要缩放，默认是 1f
        float tagMultiplier = ProjectileID.Sets.SummonTagDamageMultiplier[projectile.type];

        if (npc.HasBuff<MyWhipDebuff>()) {
            modifiers.FlatBonusDamage += MyWhipDebuff.TagDamage * tagMultiplier;
        }
    }
}
```

## 5. 让攻击被识别为「随从 / 哨兵相关」

| 内容 | 必设 |
| --- | --- |
| 随从本体 | `Projectile.minion = true;`、`Projectile.minionSlots = 1f;`、`ProjectileID.Sets.MinionSacrificable[Type] = true;` |
| 随从射击 | `ProjectileID.Sets.MinionShot[Type] = true;` |
| 哨兵本体 | `Projectile.sentry = true;`、`ProjectileID.Sets.MinionTargettingFeature[Type] = true;` |
| 哨兵射击 | `ProjectileID.Sets.SentryShot[Type] = true;` |

> 未登记 `MinionShot` / `SentryShot` 时，`projectile.IsMinionOrSentryRelated` 为假，鞭子 tag 加成会**完全失效**。

## 6. 常见错误

| 错误 | 后果 |
| --- | --- |
| 随从射击没登记 `MinionShot` | 鞭子 tag 伤害不生效 |
| 哨兵射击没登记 `SentryShot` | 同上 |
| tag 增益没设 `IsATagBuff` | 对免疫减益的敌人无效 / 其它系统不识别 |
| `GlobalNPC` 没检查 `IsMinionOrSentryRelated` | 玩家其它伤害也吃到 tag 加成 |
| 鞭子没设 `IsAWhip` | 命中判定 / 药剂效果异常 |
