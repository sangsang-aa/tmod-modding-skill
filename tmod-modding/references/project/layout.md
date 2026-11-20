# 目录与命名规范

> **总原则：工程文件排布以 `ExampleMod` 为基准模板。**
> 新增内容前，先在 `ExampleMod` 中找到同类实现，照它的目录层级、子目录划分、文件与贴图命名来放。不要自创目录结构。

## 1. 基本规则

- 命名空间 = 模组名（例 `TutorialMod`），内容类放在 `Content/<类别>/` 下。
- 一个内容类一个 `.cs` 文件，**文件名与类名一致**。
- 贴图与该 `.cs` 放在同一目录且同名（`.png`）。
- 同类内容多时，按子类型再分层。

## 2. 子目录分层（对照 ExampleMod）

- `Content/Items/` → `Weapons/`、`Armor/`、`Accessories/`、`Ammo/`、`Consumables/`、`Tools/`、`Placeable/`、`Mounts/`
- `Content/Projectiles/` → `Minions/`、`Rockets/` 等
- `Content/NPCs/` → `MinionBoss/`、`TownPets/`、`Gores/` 等
- 少量/独立/展示类内容可直接放在类型根目录（如 `Content/Items/ExampleItem.cs`）。

## 3. `Common/` 分组

按「作用对象」分组，与 ExampleMod 一致：

`GlobalItems/`、`GlobalNPCs/`、`GlobalProjectiles/`、`GlobalTiles/`、`GlobalWalls/`、`GlobalBuffs/`、`GlobalBossBars/`、`Players/`、`Systems/`、`Configs/`、`UI/`、`Commands/`、`ItemDropRules/`、`EntitySources/`

## 4. 其他约定

- 跨类型共享素材放 `Assets/`（`Textures/`、`Sounds/`、`Music/`、`Effects/`），不要和内容混放。
- 辅助类（如蠕虫身节 `Worm.cs`、配套全局类）与它服务的内容放在同一目录。
- 新增内容按类型放进对应目录，不要全部堆在根目录。

## 5. 参考结构

```text
<ModName>/
├── build.txt
├── <ModName>.cs               # Mod 主类
├── <ModName>.csproj
├── Content/
│   ├── Items/
│   │   ├── Weapons/  Armor/  Accessories/  Ammo/  Tools/  Placeable/ ...
│   │   └── <独立内容>.cs / .png
│   ├── Projectiles/  (Minions/ …)
│   ├── NPCs/         (MinionBoss/ TownPets/ Gores/ …)
│   ├── Tiles/  Walls/
│   ├── Buffs/  Dusts/
│   └── ...
├── Common/
│   ├── GlobalItems/  GlobalNPCs/  GlobalProjectiles/
│   ├── GlobalTiles/  GlobalWalls/ GlobalBuffs/
│   ├── Players/  Systems/  Configs/  UI/  Commands/
│   └── ...
├── Assets/
│   ├── Textures/  Sounds/  Music/  Effects/
├── Localization/
│   └── en-US_Mods.<ModName>.hjson
└── Properties/
```

## 6. 贴图与附加资源命名

- 主贴图：与类同名，如 `ExampleSword.cs` + `ExampleSword.png`。
- 附加贴图：`<类名>_<后缀>.png`，与类文件同目录：
  - 发光/高亮：`ExampleHeldProjectileWeapon_Glow.png`、`ExampleInteractableProjectile_Highlight.png`
  - 多部件：`ExampleWormHead.png` / `ExampleWormBody.png` / `ExampleWormTail.png`
  - 城镇 NPC 头像/派对变体：`ExamplePerson_Head.png`、`ExamplePerson_Party.png`
- 缺贴图时游戏会用占位图并可能报错；新增内容前先与用户确认贴图来源与命名。

## 7. 速查表

| 内容 | 存放目录（仿 ExampleMod） | 贴图 |
| --- | --- | --- |
| 武器 | `Content/Items/Weapons/` | `Foo.png`（同名） |
| 饰品 / 盔甲 / 弹药 | `Content/Items/Accessories/` 等 | `Foo.png` |
| 弹幕（含随从） | `Content/Projectiles/`（随从放 `Minions/`） | `Foo.png` |
| 敌怪 / Boss | `Content/NPCs/`（Boss 可建 `XxxBoss/`） | `Foo.png`；多部位 `FooHead/Body/Tail.png` |
| 方块 / 墙 | `Content/Tiles/`、`Content/Walls/` | 同名 `.png`，多帧命名见 Wiki |
| 发光/高亮附加图 | 与主类同目录 | `Foo_Glow.png`、`Foo_Highlight.png` |
| 全局类 / 系统 | `Common/GlobalItems/`、`Common/Systems/` 等 | — |
| 共享素材 | `Assets/Textures/`、`Assets/Sounds/` 等 | — |
