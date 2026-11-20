# 物品 / 武器（ModItem）

## 1. 基础近战武器

```csharp
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

namespace MyMod.Content.Items.Weapons;

public class ExampleSword : ModItem
{
    public override void SetDefaults()
    {
        Item.width = 40;
        Item.height = 40;
        Item.damage = 20;
        Item.DamageType = DamageClass.Melee;
        Item.useTime = 20;
        Item.useAnimation = 20;
        Item.useStyle = ItemUseStyleID.Swing;
        Item.knockBack = 5f;
        Item.value = Item.buyPrice(silver: 1);
        Item.rare = ItemRarityID.Blue;
        Item.UseSound = SoundID.Item1;
        Item.autoReuse = true;
    }

    public override void AddRecipes()
    {
        Recipe recipe = CreateRecipe();
        recipe.AddIngredient(ItemID.DirtBlock, 10);
        recipe.AddTile(TileID.WorkBenches);
        recipe.Register();
    }
}
```

## 2. 其他类型

- 魔法武器：`Item.DefaultToMagicWeapon(ProjectileID.X, ...)`
- 远程武器：`Item.DefaultToRangedWeapon(...)`
- 投掷武器：`Item.DefaultToThrowingWeapon(...)`
- 饰品：`Item.DefaultToAccessory(...)` + `UpdateAccessory`
- 可放置物品：`Item.DefaultToPlaceableTile(...)`
- 鞭子：`Item.DefaultToWhip(...)`，另见 `../content/summon/whip-tag-bonuses.md`

饰品示例：

```csharp
public class ExampleAccessory : ModItem
{
    public override void SetDefaults()
    {
        Item.width = 24;
        Item.height = 24;
        Item.accessory = true;
        Item.value = Item.sellPrice(gold: 1);
        Item.rare = ItemRarityID.Green;
    }

    public override void UpdateAccessory(Player player, bool hideVisual)
    {
        player.moveSpeed += 0.1f;
    }
}
```

> 盔甲涉及装备槽与贴图规则，直接参考 `ExampleMod/Content/Items/Armor/`，不要凭记忆写。

## 3. 关键约定

- `DamageType` 必须设置；否则会被当作真伤，饰品 / 药水加成失效。
- 召唤类伤害不要硬编码，见 `../content/summon/equipment-scaling.md`。
- 显示名 / Tooltip 写 hjson，不在代码里设置。
- 武器需要弹幕时，`Item.shoot` 指向弹幕类型，并确认 `Item.noMelee` / `useStyle`。

## 4. 本地化键

```hjson
Items: {
    ExampleSword: {
        DisplayName: Example Sword
        Tooltip: A basic sword.
    }
}
```

## 5. 自检

- [ ] `DamageType` 已设置。
- [ ] 文件名 = 类名，贴图同名同目录。
- [ ] 显示名 / Tooltip 在 hjson。
- [ ] 配方使用 `CreateRecipe()` 并 `Register()`。
- [ ] 若为召唤类，已对照 `../content/summon/index.md`。
