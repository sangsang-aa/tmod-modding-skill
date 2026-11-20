# 配方（Recipe）

## 1. 基础配方

```csharp
public override void AddRecipes()
{
    CreateRecipe()
        .AddIngredient(ItemID.DirtBlock, 10)
        .AddTile(TileID.WorkBenches)
        .Register();
}
```

- 也可用 `Recipe.CreateRecipe()` 在 `ModSystem` 中注册。
- 链式调用后必须 `Register()`。

## 2. 常用写法

```csharp
CreateRecipe()
    .AddIngredient<MyItem>(5)
    .AddIngredient(ItemID.Gel, 10)
    .AddTile(TileID.Anvils)
    .AddCondition(Condition.InGraveyard) // 可选条件
    .Register();
```

## 3. 配方组

```csharp
CreateRecipe()
    .AddRecipeGroup(RecipeGroupID.Wood, 10)
    .AddTile(TileID.WorkBenches)
    .Register();
```

- 原版配方组：`RecipeGroupID.Wood`、`RecipeGroupID.IronBar` 等。
- 自定义配方组：在 `ModSystem` 中注册 `RecipeGroup`；参考 `ExampleMod/Content/ExampleRecipes.cs`。

## 4. 自定义合成站

- 用 `ModTile` 的 `AddMapEntry` / `TileObjectData` 建站，配方中 `.AddTile(ModContent.TileType<MyStation>())`。
- 参考 `ExampleMod/Content/Tiles/Furniture/ExampleWorkbench.cs`。

## 5. 关键约定

- 配方中的物品/方块用 `ModContent.ItemType<T>()` / `ModContent.TileType<T>()`。
- 新增配方前先确认是否已有同类配方，避免重复。
- 修改原版配方需谨慎；优先新增而不是覆盖。

## 6. 自检

- [ ] 已调用 `Register()`。
- [ ] 材料数量、合成站正确。
- [ ] 自定义配方组已在 `ModSystem` 注册。
- [ ] 没有与现有配方冲突。
