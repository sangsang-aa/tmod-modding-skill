# 弹幕（ModProjectile）

## 1. 基础弹幕

```csharp
using Terraria;
using Terraria.ModLoader;

namespace MyMod.Content.Projectiles;

public class ExampleProjectile : ModProjectile
{
    public override void SetDefaults()
    {
        Projectile.width = 16;
        Projectile.height = 16;
        Projectile.friendly = true;
        Projectile.DamageType = DamageClass.Melee;
        Projectile.penetrate = 1;
        Projectile.timeLeft = 300;
    }

    public override void AI()
    {
        Projectile.rotation += 0.2f;
        Projectile.velocity.Y += 0.1f; // 示例：重力
    }
}
```

## 2. 从物品发射

```csharp
Projectile.NewProjectile(
    Item.GetSource_FromThis(),
    position,
    velocity,
    ModContent.ProjectileType<ExampleProjectile>(),
    damage,
    knockback,
    Owner);
```

- 必须使用带 `IEntitySource` 的重载。
- 从弹幕 AI 中生成，用 `projectile.GetSource_FromAI()`。

## 3. 常用属性

| 属性 | 作用 |
| --- | --- |
| `friendly` / `hostile` | 是否伤害敌人 / 玩家 |
| `DamageType` | 伤害类型；不设会变真伤 |
| `penetrate` | 穿透次数；`-1` = 不消失 |
| `timeLeft` | 存活帧数 |
| `tileCollide` | 是否碰地形 |
| `ignoreWater` | 是否忽略水 |
| `usesLocalNPCImmunity` + `localNPCHitCooldown` | 每只弹幕独立命中冷却 |
| `aiStyle = 0` | 不使用原版 AI，自己写 |

## 4. 索敌与追踪

- 用 `NPC.CanBeChasedBy()` 过滤目标。
- 用 `Collision.CanHitLine(...)` 做视线检查。
- 平滑追踪：`velocity = (velocity * (inertia - 1) + direction * speed) / inertia;`
- 召唤类优先索敌见 `../content/summon/whip-targeting.md`。

## 5. 网络同步

- 自定义状态放在 `Projectile.ai[]`（会随弹幕同步），或写 `SendExtraAI` / `ReceiveExtraAI`。
- 状态改变后设置 `Projectile.netUpdate = true`。
- 详见 `../coding/common-apis.md` §4。

## 6. 贴图

- 默认使用与类同名的 `.png`。
- 复用原版贴图：`public override string Texture => "Terraria/Images/Projectile_" + ProjectileID.Xxx;`

## 7. 自检

- [ ] `DamageType` 已设置。
- [ ] 生成时带 `IEntitySource`。
- [ ] 自定义状态已同步（`ai[]` 或 `SendExtraAI`）。
- [ ] 召唤类已登记 `MinionShot` / `SentryShot`（见 `../content/summon/index.md`）。
