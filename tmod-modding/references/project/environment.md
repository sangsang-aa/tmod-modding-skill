# 工作环境

## 路径

- 模组源码根目录：`C:\Users\zh180\Documents\My Games\Terraria\tModLoader\ModSources`
- 每个模组是一个独立子文件夹（如 `TutorialMod`、`ExampleMod`）。
- `tModLoader.targets` 指向本机 tModLoader 安装目录。

## 版本

- 目标：**tModLoader 1.4.4 (stable)**。
- 不要使用 1.3 或 1.4.5 的过时/未发布 API。
- 当前工程实际构建目标以 `bin/Release/net8.0` 为准（本机环境）。

## 参考

- `ExampleMod`：官方全量示例，**写新内容前优先在其中找同类实现**。
- `ExampleMod/Content/**`：内容类示例。
- `ExampleMod/Common/**`：全局类、系统、配置示例。
- Wiki：<https://github.com/tModLoader/tModLoader/wiki>

## 语言约定

- 说明、注释、文档用中文。
- 类型名、方法名、原版 ID 常量保持英文原样。
