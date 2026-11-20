# 方块 / 墙（ModTile / ModWall）

## 1. 基础方块

```csharp
using Microsoft.Xna.Framework;
using Terraria;
using Terraria.ModLoader;

namespace MyMod.Content.Tiles;

public class ExampleBlock : ModTile
{
    public override void SetStaticDefaults()
    {
        Main.tileSolid[Type] = true;
        Main.tileMergeDirt[Type] = true;
        Main.tileBlockLight[Type] = true;
        AddMapEntry(new Color(120, 90, 60));
        MinPick = 25;
    }

    public override void NumDust(int i, int j, bool fail, ref int num)
        => num = fail ? 1 : 3;
}
```

## 2. 基础墙

```csharp
using Microsoft.Xna.Framework;
using Terraria;
using Terraria.ModLoader;

namespace MyMod.Content.Walls;

public class ExampleWall : ModWall
{
    public override void SetStaticDefaults()
    {
        Main.wallHouse[Type] = true;
        AddMapEntry(new Color(80, 60, 40));
    }
}
```

## 3. 常见设置

| 设置 | 作用 |
| --- | --- |
| `Main.tileSolid[Type]` | 是否实心 |
| `Main.tileMergeDirt[Type]` | 是否与土块融合 |
| `Main.tileBlockLight[Type]` | 是否挡光 |
| `Main.tileLighted[Type]` | 是否发光 |
| `AddMapEntry(Color)` | 地图颜色 |
| `MinPick` | 最低镐力 |
| `DustType` | 挖掘尘土 |
| `HitSound` | 挖掘音效 |

## 4. 多格结构 / 家具

- 多格家具需要 `TileObjectData`，并处理 `KillMultiTile` / `Drop`。
- 直接参考 `ExampleMod/Content/Tiles/` 下同类实现，不要凭记忆写。
- 家具、火把、平台等都有既定的 `TileObjectData` 模板。

## 5. 掉落

```csharp
public override void Drop(int i, int j)
{
    // 默认会掉落自身；自定义掉落时在此处理
}
```

## 6. 自检

- [ ] `AddMapEntry` 已设置，地图不会显示异常。
- [ ] 贴图命名符合多帧 / 多格规则。
- [ ] `MinPick`、`HitSound`、`DustType` 合理。
- [ ] 多格结构已处理放置/破坏。
