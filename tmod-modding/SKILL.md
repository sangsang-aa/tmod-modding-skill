---
name: tModLoader 模组开发
description: tModLoader (Terraria) 模组开发的入口与导航。任何改动前必须先说明逻辑并等待用户确认；按任务路由到 references/ 下的 workflow、project、coding、content、localization、build 子文档。Use for tModLoader / Terraria mod development in ModSources.
---

# tModLoader 模组开发（总览）

本文件是**入口与导航**，不承载全部细节。处理任务时：先读 §0，再按 §2 路由到对应文档。

## 0. 铁律（最高优先级）

1. 任何开发 / 修改 / 删除任务，**先说明实现逻辑与可选方向，等用户明确确认后再执行**。
2. 确认前只允许**只读**操作（读文件、搜索、查 `ExampleMod`、看日志）。
3. 执行中若需偏离已确认方案，**立即停下并再次确认**。

- 完整方案模板与确认判定：`references/workflow/confirmation.md`
- 完整任务步骤与汇报格式：`references/workflow/task-flow.md`

## 1. 环境（摘要）

- 根目录：`C:\Users\zh180\Documents\My Games\Terraria\tModLoader\ModSources`
- 每个模组一个子文件夹；以 `ExampleMod` 为参考。
- 目标：**tModLoader 1.4.4 stable**，不要用 1.3 / 1.4.5 的过时 API。
- 详见：`references/project/environment.md`

## 2. 任务路由表

| 你要做的事 | 先读 |
| --- | --- |
| 开工前确认、方案模板 | `references/workflow/confirmation.md` |
| 澄清 → 勘察 → 实现 → 自检 → 汇报 | `references/workflow/task-flow.md` |
| 新文件放哪、目录 / 贴图命名 | `references/project/layout.md` |
| 写 C#、生命周期、常用 API | `references/coding/standards.md`、`references/coding/common-apis.md` |
| 物品 / NPC / 弹幕 / 方块 / 增益 / 配方 / 全局类 | `references/content/index.md` |
| 随从 / 哨兵 / 鞭子 / 召唤加成 | `references/content/summon/index.md` |
| 显示名 / Tooltip / hjson | `references/localization.md` |
| 构建、打包、排错 | `references/build/build.md`、`references/build/troubleshooting.md` |
| 写 / 改注释 | 技能 `tmod-code-comments` |

## 3. 每次任务必做清单

- [ ] 已按 `references/workflow/confirmation.md` 说明方案并获得确认。
- [ ] 目录、文件名、贴图排布对照 `ExampleMod`（`references/project/layout.md`）。
- [ ] 显示名 / Tooltip 未硬编码，写在 hjson（`references/localization.md`）。
- [ ] 召唤类满足 `references/content/summon/index.md` 的全部强制项。
- [ ] 伤害 / 击退未硬编码：设 `DamageType`，用 `GetTotalDamage` / `GetKnockback`。
- [ ] 新增贴图前已与用户确认来源与命名。
- [ ] 构建验证后再汇报；失败时如实说明（`references/build/build.md`）。

## 4. 文档分层

```text
SKILL.md                         ← 你在这里：总览 + 路由
references/
├── workflow/                    ← 确认流程、任务步骤
├── project/                     ← 环境、目录与命名
├── coding/                      ← 编码总则、常用 API
├── content/                     ← 按内容类型拆分
│   └── summon/                  ← 召唤：总则 + 三个技术细项
├── localization.md              ← hjson
└── build/                       ← 构建、排错
```

## 5. 参考

- 官方示例：`ExampleMod/Content/**`、`ExampleMod/Common/**`
- Wiki：<https://github.com/tModLoader/tModLoader/wiki>
