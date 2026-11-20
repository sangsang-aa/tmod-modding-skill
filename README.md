# tModLoader Modding Skills

[中文](#中文) | [English](#english)

## 中文

面向 tModLoader (Terraria) 模组开发的技能文档集合，供 OpenCode 等编码代理加载使用。

### 包含内容

- `tmod-modding/` — 模组开发主技能
  - `SKILL.md`：总览、确认流程、任务路由、必做清单
  - `references/workflow/`：先说明后确认、标准任务流程
  - `references/project/`：环境、目录排布与命名
  - `references/coding/`：编码规范、常用 API
  - `references/content/`：物品、NPC、弹幕、方块、增益、配方、全局类
  - `references/content/summon/`：随从、哨兵、鞭子、召唤加成
  - `references/localization.md`、`references/build/`：本地化、构建与排错
- `tmod-code-comments/` — 代码注释规范：解释钩子调用时机、原版 ID，并附已验证的 WIKI/API 链接

### 核心原则

1. **先说明，后确认，再执行**：任何改动前先给出实现逻辑与可选方向，等待明确确认。
2. **目录仿照 ExampleMod**：文件、贴图、子目录按官方示例排布。
3. **召唤类必须完整**：鞭子优先索敌、tag 加成、饰品/药水/栏位加成不可绕过。
4. **注释面向看不到原版源码的读者**：解释钩子语义并附文档链接。

### 使用方式

将 `tmod-modding/` 与 `tmod-code-comments/` 放入 OpenCode 项目的 `.opencode/skills/` 目录即可被自动发现。

## English

A collection of skills for tModLoader (Terraria) mod development, intended for coding agents such as OpenCode.

- `tmod-modding/` — main modding skill: confirmation-first workflow, project layout, coding standards, content templates, summon/whip rules, localization, and build.
- `tmod-code-comments/` — commenting standard for explaining hooks, vanilla IDs, and linking to verified wiki/API docs.

### Core rules

1. Explain the implementation plan and wait for explicit confirmation before changing code.
2. Mirror ExampleMod for file, folder, and texture naming.
3. Summon content must include whip targeting/bonuses and must not bypass accessory/potion scaling.
4. Comments are written for readers who cannot see vanilla source.
