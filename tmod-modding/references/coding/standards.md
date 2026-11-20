# 编码规范（总则）

## 1. 类与文件

- 每个内容类单独 `public class Xxx : ModItem / ModNPC / ModProjectile / ModTile / ModBuff / ModPlayer / GlobalItem / GlobalNPC / ModSystem`。
- 文件名与类名一致；贴图同名同目录。

## 2. 生命周期分工

- `SetStaticDefaults()`：注册阶段调用一次，写静态数据（帧数、`Sets`、注册表）。
- `SetDefaults()`：实例初始化，写该实例的默认属性。
- 不要把实例逻辑塞进 `SetStaticDefaults`。

## 3. 文本

- 显示名 / Tooltip **不要硬编码**，写在 `Localization/en-US_Mods.<ModName>.hjson`。
- 动态 Tooltip 才用 `ModifyTooltips(List<TooltipLine>)` 追加。
- 详见 `../localization.md`。

## 4. 常量与辅助方法

- 优先使用 vanilla 常量与辅助方法：
  - `DamageClass.Melee/Ranged/Magic/Summon/Throwing`
  - `ItemRarityID.*`、`ItemUseStyleID.*`、`SoundID.*`、`ProjectileID.*`、`TileID.*`、`ItemID.*`、`NPCID.*`
  - `Item.DefaultToMagicWeapon(...)`、`Item.DefaultToRangedWeapon(...)`、`Item.DefaultToAccessory(...)`、`Item.DefaultToPlaceableTile(...)`、`Item.CloneDefaults(ItemID.…)`
- 常用 API 与钩子见 `common-apis.md`。

## 5. 生成与配方

- 弹幕生成使用带 `IEntitySource` 的重载，例如 `Projectile.NewProjectile(Item.GetSource_FromThis(), ...)`。
- 配方用 `CreateRecipe()` / `Recipe.CreateRecipe()` 链式写法：`AddIngredient` + `AddTile` + `Register`。
- 详见 `../content/recipes.md`。

## 6. 资源

- 需要新增贴图时，先说明「需要用户提供/需生成哪些 png」，不要假装贴图存在。
- 贴图与附加图命名见 `../project/layout.md` §6。

## 7. 注释

- 写或改代码时按专用技能 `tmod-code-comments` 执行：
  - 说明每个钩子的调用时机与原版行为；
  - 标注涉及的原版 ID；
  - 附已验证的 WIKI / API 链接。

## 8. 禁止

- 不修改 `ExampleMod` 来「试验」，示例文件只读。
- 不使用 1.3 / 1.4.5 的过时 API。
- 不硬编码最终伤害/击退（召唤类尤其严格，见 `../content/summon/equipment-scaling.md`）。
