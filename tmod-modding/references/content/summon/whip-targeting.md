# 鞭子的优先索敌

> 上级：`index.md`。本文件只讲「随从 / 哨兵如何优先攻击鞭子标记的目标」。

## 1. 原理

- 鞭子命中敌人时，把该敌人写入 `Player.MinionAttackTargetNPC`。
- 随从 / 哨兵 / 召唤射击索敌时，**先读 `Projectile.OwnerMinionAttackTargetNPC`**，没有再找最近的敌人。
- 玩家右键指定目标也走同一套机制；随从需登记 `MinionTargettingFeature` 才支持右键。

## 2. 鞭子命中时写入标记

```csharp
public override void OnHitNPC(NPC target, NPC.HitInfo hit, int damageDone)
{
    // 施加鞭子专属 tag 增益（供 GlobalNPC 加成使用）
    target.AddBuff(ModContent.BuffType<MyWhipDebuff>(), 240);

    // 关键：让所有随从/哨兵优先攻击这个目标
    Main.player[Projectile.owner].MinionAttackTargetNPC = target.whoAmI;
}
```

## 3. 随从 / 哨兵的索敌顺序

```csharp
private static bool TryGetTarget(Projectile projectile, float range, out NPC target, out float distance)
{
    // ① 鞭子/右键标记的目标，永远优先
    target = projectile.OwnerMinionAttackTargetNPC;
    if (target != null && target.CanBeChasedBy()) {
        distance = Vector2.Distance(target.Center, projectile.Center);
        return true;
    }

    // ② 没有标记时，才找最近的合法敌人
    target = null;
    distance = range;
    foreach (var npc in Main.ActiveNPCs) {
        if (!npc.CanBeChasedBy()) {
            continue;
        }

        float d = Vector2.Distance(npc.Center, projectile.Center);
        if (d < distance) {
            distance = d;
            target = npc;
        }
    }

    return target != null;
}
```

> 等价写法：`owner.HasMinionAttackTargetNPC` + `Main.npc[owner.MinionAttackTargetNPC]`（见 `ExampleMod/Content/Projectiles/Minions/ExampleSimpleMinion.cs`）。

## 4. 右键标记支持

```csharp
public override void SetStaticDefaults()
{
    ProjectileID.Sets.MinionTargettingFeature[Type] = true; // 允许右键指定目标
}
```

## 5. 常见错误

| 错误 | 后果 |
| --- | --- |
| 只找最近敌人，不读 `OwnerMinionAttackTargetNPC` | 鞭子打了 A，随从却去打更近的 B |
| 鞭子 `OnHitNPC` 没写 `MinionAttackTargetNPC` | 随从完全不理会鞭子标记 |
| 没设 `MinionTargettingFeature` | 右键指定目标无效 |
| 读 `Main.npc[owner.MinionAttackTargetNPC]` 前没判断 `HasMinionAttackTargetNPC` | 可能取到无效索引 |
