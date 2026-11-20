# 玩家 / 全局类 / 系统（ModPlayer / Global* / ModSystem）

## 1. 何时用哪种

| 类型 | 用途 |
| --- | --- |
| `ModPlayer` | 玩家身上的状态、属性修正、存档字段 |
| `GlobalItem` | 对所有物品（含原版）生效的钩子 |
| `GlobalNPC` | 对所有 NPC（含原版）生效的钩子 |
| `GlobalProjectile` | 对所有弹幕（含原版）生效的钩子 |
| `GlobalBuff` | 对所有增益（含原版）生效的钩子 |
| `ModSystem` | 全局加载/保存/网络/配方/绘制层等 |

## 2. ModPlayer

```csharp
public class ExamplePlayer : ModPlayer
{
    public override void ResetEffects()
    {
        // 每帧重置临时属性，避免叠加
    }

    public override void UpdateEquip(Player player)
    {
        // 装备效果
    }
}
```

- 需要存档的字段用 `SaveData` / `LoadData`。
- 多人同步字段用 `[SyncVar]` 或 `SendClientChanges`。

## 3. GlobalItem

```csharp
public class ExampleGlobalItem : GlobalItem
{
    public override bool AppliesToEntity(Item entity, bool lateInstantiation)
        => entity.type == ItemID.ZephyrFish;

    public override void ModifyTooltips(Item item, List<TooltipLine> tooltips)
    {
        // 增删改提示行
    }
}
```

- `AppliesToEntity` 用来缩小作用范围，避免影响所有物品。

## 4. GlobalNPC

```csharp
public class ExampleGlobalNPC : GlobalNPC
{
    public override void OnKill(NPC npc)
    {
        // 追加掉落等
    }

    public override void ModifyHitByProjectile(NPC npc, Projectile projectile, ref NPC.HitModifiers modifiers)
    {
        // 改命中结算（鞭子 tag 加成等）
    }
}
```

## 5. GlobalProjectile

```csharp
public class ExampleGlobalProjectile : GlobalProjectile
{
    public override bool InstancePerEntity => true;

    public override bool AppliesToEntity(Projectile entity, bool lateInstantiation)
        => entity.type == ProjectileID.ZephyrFish;

    public override void PostAI(Projectile projectile)
    {
        // 在原版 AI 之后追加逻辑
    }
}
```

- 需要每个实体单独状态时，`InstancePerEntity => true`。

## 6. ModSystem

```csharp
public class ExampleSystem : ModSystem
{
    public override void PostUpdateWorld()
    {
        // 世界每帧逻辑
    }
}
```

- 也用于 `AddRecipes`、`Load` / `SaveWorldData`、网络包注册、绘制层等。

## 7. 自检

- [ ] `AppliesToEntity` 已缩小范围（Global 类）。
- [ ] 存档/同步字段已处理。
- [ ] 全局逻辑没有误伤其它内容。
