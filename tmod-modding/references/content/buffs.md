# 增益 / 减益（ModBuff）

## 1. 基础增益

```csharp
using Terraria;
using Terraria.ModLoader;

namespace MyMod.Content.Buffs;

public class ExampleBuff : ModBuff
{
    public override void Update(Player player, ref int buffIndex)
    {
        player.moveSpeed += 0.2f;
    }
}
```

## 2. 常用设置

| 设置 | 作用 |
| --- | --- |
| `Main.buffNoSave[Type]` | 退出世界后不保存 |
| `Main.buffNoTimeDisplay[Type]` | 不显示剩余时间 |
| `Main.debuff[Type]` | 标记为减益 |
| `BuffID.Sets.IsATagBuff[Type]` | 鞭子 tag 增益（见召唤文档） |

## 3. 宠物 / 随从增益模式

宠物或随从通常靠一个“隐形增益”维持存在：

```csharp
public override void SetStaticDefaults()
{
    Main.buffNoSave[Type] = true;
    Main.buffNoTimeDisplay[Type] = true;
}

public override void Update(Player player, ref int buffIndex)
{
    if (player.ownedProjectileCounts[ModContent.ProjectileType<MyPet>()] > 0) {
        player.buffTime[buffIndex] = 18000; // 有宠物就续时
    }
    else {
        player.DelBuff(buffIndex); // 没宠物就移除
        buffIndex--;
    }
}
```

## 4. 关键约定

- 增益说明写 hjson：`Buffs.<类名>.DisplayName` / `.Description`。
- 减益若需要影响命中结算，放在 `GlobalNPC.ModifyHitBy*`，不要塞进 `Update`。
- 鞭子 tag 增益必须设 `BuffID.Sets.IsATagBuff[Type] = true;`，详见 `../content/summon/whip-tag-bonuses.md`。

## 5. 自检

- [ ] 宠物/随从增益已设置 `buffNoSave` / `buffNoTimeDisplay`。
- [ ] `Update` 中正确续时或移除。
- [ ] 显示名与说明在 hjson。
- [ ] 减益的命中效果没有放错钩子。
