# 常见错误排查

## 1. 编译 / 构建

| 现象 | 常见原因 |
| --- | --- |
| 编译错误：找不到类型 | `using` 缺失或命名空间写错 |
| `CS0169` 字段未使用 | 编译已通过；清理未使用字段即可 |
| `TML003` / `MSB3073` | tModLoader 正在运行或模组已加载；关闭后重试 |
| 使用了过时 API | 用了 1.3 或 1.4.5 写法，改用 1.4.4 stable API |
| 改了代码但游戏行为没变 | 没有 Build + Reload；或改的是错误的类 |

## 2. 内容不显示 / 显示异常

| 现象 | 常见原因 |
| --- | --- |
| 内容完全不出现 | 类未继承正确基类 / `SetDefaults` 未设置关键字段 |
| 显示名为类名 | hjson 缺少对应 `DisplayName` 条目 |
| 贴图紫黑占位 | `.png` 缺失或路径 / 命名不匹配 |
| 贴图错位 / 朝向错误 | 贴图尺寸、`DrawOffsetX`、`spriteDirection`、旋转基点不对 |
| 方块地图颜色异常 | 未调用 `AddMapEntry` |

## 3. 运行时

| 现象 | 常见原因 |
| --- | --- |
| 弹幕不伤害敌人 | 未设 `friendly` / `DamageType`，或 `MinionContactDamage` 未返回 true |
| 鞭子标记无效 | 未写 `MinionAttackTargetNPC` / 未登记 `MinionTargettingFeature` |
| 鞭子 tag 无加成 | 射击未登记 `MinionShot` / `SentryShot`，或 tag 增益未设 `IsATagBuff` |
| 换饰品伤害不变 | 伤害被硬编码，未走 `GetTotalDamage` |
| 召唤栏位异常 | 硬编码数量，未用 `maxMinions` / `minionSlots` |
| 多人不同步 | 用了普通字段而非 `[SyncVar]` / 未在 `SendExtraAI` 发送 |

## 4. 排查顺序

1. 看构建输出：先区分编译错误 / 打包环境错误。
2. 看日志：`tModLoader` 日志中定位异常堆栈。
3. 最小化复现：临时禁用可疑内容，确认问题来源。
4. 对照 `ExampleMod` 同类实现，逐项比较注册、贴图、本地化。
5. 仍无法定位时，向用户说明已排查项与下一步计划，不要盲目大改。
