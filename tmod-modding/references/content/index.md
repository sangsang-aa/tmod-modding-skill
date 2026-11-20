# 内容类型索引

> 先按 `../project/layout.md` 决定文件放哪，再进入对应模板。所有模板都只是骨架，使用前仍须走 `../workflow/confirmation.md`。

| 要做什么 | 文档 |
| --- | --- |
| 物品、武器、工具、饰品、盔甲、弹药、消耗品 | `items.md` |
| 敌怪、Boss、城镇 NPC、小动物 | `npcs.md` |
| 弹幕、飞行物、法术弹、随从射击 | `projectiles.md` |
| 方块、墙、家具、多格结构 | `tiles-and-walls.md` |
| 增益 / 减益 | `buffs.md` |
| 合成配方、配方组、合成条件 | `recipes.md` |
| ModPlayer / GlobalItem / GlobalNPC / GlobalProjectile / ModSystem | `global-classes.md` |
| 随从、哨兵、鞭子、召唤伤害加成 | `summon/index.md` |

## 通用约定

- 显示名 / Tooltip 一律写 hjson：`../localization.md`。
- 每个内容类单独一个文件，文件名 = 类名。
- 贴图与类同名同目录；附加图用 `<类名>_<后缀>.png`。
- 需要贴图但用户未提供时，先说明，不要假设文件存在。

## 召唤类特别提醒

只要内容涉及召唤，**必须**额外满足 `summon/index.md` 的强制清单：

- 鞭子优先索敌。
- 鞭子 tag 加成。
- 饰品 / 药水 / 召唤栏位加成不被绕过。
