# 敌怪 / Boss / 城镇 NPC（ModNPC）

## 1. 基础敌怪

```csharp
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

namespace MyMod.Content.NPCs;

public class ExampleEnemy : ModNPC
{
    public override void SetStaticDefaults()
    {
        Main.npcFrameCount[Type] = 1;
        // NPCID.Sets.* 相关设置
    }

    public override void SetDefaults()
    {
        NPC.width = 32;
        NPC.height = 32;
        NPC.lifeMax = 100;
        NPC.damage = 20;
        NPC.defense = 5;
        NPC.value = 100f;
        NPC.aiStyle = NPCAIStyleID.Fighter;
        NPC.HitSound = SoundID.NPCHit1;
        NPC.DeathSound = SoundID.NPCDeath1;
    }

    public override void AI() { }

    public override float SpawnChance(NPCSpawnInfo spawnInfo)
        => spawnInfo.Player.ZoneOverworldHeight ? 0.1f : 0f;

    public override void NPCLoot()
    {
        Item.NewItem(NPC.GetSource_Loot(), NPC.getRect(), ItemID.Gel, Main.rand.Next(1, 3));
    }
}
```

## 2. Boss 要点

- `NPC.boss = true;`，并设置 `NPC.value`、`Music`、`NPC.BossBar`（如需要）。
- 掉落写在 `NPCLoot()`；宝物袋 / 专属掉落可参考 `ExampleMod` 的 Boss 示例。
- 多人同步：AI 中的随机/状态变更要放在 owner 端并 `netUpdate`，或使用 `NPC.netUpdate`。
- 血条、阶段、召唤小怪等复杂逻辑，优先在 `ExampleMod/Content/NPCs/` 找同类实现。

## 3. 城镇 NPC / 小动物

- 城镇 NPC 需要 `NPC.townNPC = true;`、`NPC.friendly = true;`，并处理房屋/对话。
- 小动物通常 `NPC.friendly = true;` 且不主动攻击。
- 具体模板见 `ExampleMod/Content/NPCs/ExamplePerson.cs`、`ExampleCritterNPC.cs`。

## 4. 关键约定

- 1.4.4 使用 `NPCAIStyleID.*`，不要用旧的整数 AI 常量。
- `SetStaticDefaults` 写帧数 / `Sets`；`SetDefaults` 写实例属性。
- 显示名写 hjson：`NPCs.<类名>.DisplayName`。
- 贴图多部位时用 `FooHead.png` / `FooBody.png` / `FooTail.png`。

## 5. 自检

- [ ] `NPCAIStyleID` / 帧数 / 贴图匹配。
- [ ] 掉落写在 `NPCLoot()`，使用 `GetSource_Loot()`。
- [ ] 显示名在 hjson。
- [ ] 生成条件（`SpawnChance`）合理，且不会在错误环境刷出。
