# 构建与验证

## 1. build.txt

```ini
displayName = My Mod
author = YourName
version = 0.1
```

- 可选：`homepage`、`description`、`hideCode`、`hideResources`、`includeSource`、`buildIgnore`、`icon` 等。
- 版本号改动若与发布相关，先与用户确认。

## 2. 构建方式

1. **游戏内（首选）**：Workshop / 开发模组 → **Build + Reload**。
   - 编译失败会在界面输出错误，据此定位文件与行号。
2. **命令行**：在模组目录执行 `dotnet build`。
   - 不确定用户环境时，先问「用游戏内构建还是命令行构建」。

## 3. 常见构建阻断

| 现象 | 原因 / 处理 |
| --- | --- |
| `TML003: Please close tModLoader or disable the mod in-game to build mods directly.` | tModLoader 正在运行或模组已加载；关闭游戏 / 禁用模组后重试 |
| `MSB3073` 且伴随 TML003 | 同上，是打包阶段被游戏占用阻断，不是 C# 编译错误 |
| 只有 `warning CS0169`（字段未使用） | 编译已通过，可清理未使用字段 |

> 构建失败时先区分「C# 编译错误」与「打包 / 环境错误」，不要把环境问题误报为代码问题。

## 4. 构建后检查

- 新内容是否出现在游戏中（Item Browser / NPC 生成 / 合成界面）。
- `Localization` hjson 是否补全条目并已填写。
- 是否有贴图缺失 / 紫黑占位。
- 多人游戏是否同步正常。

## 5. 汇报格式

```text
【构建命令】...
【结果】成功 / 失败（区分编译错误与环境阻断）
【警告】列出 warning 及是否本次引入
【未验证项】明确说明
```
