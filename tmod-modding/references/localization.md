# 本地化（hjson）

tModLoader 1.4.4 用 `.hjson` 存放所有文本，**不在代码里写显示名 / Tooltip**。

## 1. 文件位置

- 英文：`Localization/en-US_Mods.<ModName>.hjson`
- 中文（可选）：`Localization/zh-Hans_Mods.<ModName>.hjson`

## 2. 键结构

以内容类型分组，类名作为键：

```hjson
Items: {
    ExampleSword: {
        DisplayName: Example Sword
        Tooltip:
            '''
            这是描述。
            这是第二行。
            '''
    }
}

NPCs: {
    ExampleEnemy: {
        DisplayName: Example Enemy
    }
}

Buffs: {
    ExampleBuff: {
        DisplayName: Example Buff
        Description: Increases movement speed.
    }
}
```

- 分组名：`Items`、`NPCs`、`Projectiles`、`Buffs`、`Tiles`、`Walls` 等。
- 动态 Tooltip 才用 `ModItem.ModifyTooltips(List<TooltipLine>)` 追加；静态文本仍放 hjson。

## 3. 自动生成

- 构建一次后，模组目录下会生成 / 更新 hjson 模板条目。
- 填入文本后重新构建生效。
- 若显示名显示为类名，通常是对应 `DisplayName` 条目缺失或键名不匹配。

## 4. 多语言注意

- `en-US` 是默认回退语言；其它语言缺失的键会回退到 `en-US`。
- 新增语言文件后，键结构要与 `en-US` 保持一致。

## 5. 自检

- [ ] 所有新内容都有 `DisplayName`。
- [ ] Tooltip / Description 已填写。
- [ ] 键名与类名完全一致（大小写敏感）。
- [ ] 没有把显示名写死在代码里。
