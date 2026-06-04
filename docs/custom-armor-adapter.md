# 自定义盔甲适配教程

本文说明如何让自定义盔甲在细节盔甲 HUD 中显示正确的护甲值、图标和半格合并效果。

## 原理

细节盔甲 HUD 渲染护甲时，需要知道四类信息：

- `itemName`：物品标识，例如 `example:ruby_helmet`
- `defense`：该装备提供的护甲值
- `slot`：装备槽位
- `materialKey`：材质合并键
- `iconTexture`：HUD 使用的图标贴图路径

同一个护甲图标可能由左右两个半格组成。如果左右半格的 `materialKey` 相同，HUD 会把它们合并成完整图标；如果不同，则按左右半格分别显示。

## 字段说明

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `itemName` | string | 是 | 自定义物品完整标识 |
| `defense` | number | 是 | 护甲值，支持半格，例如 `2.5` |
| `slot` | string | 是 | `helmet`、`chestplate`、`leggings`、`boots` |
| `materialKey` | string | 是 | 同材质合并用的稳定 key |
| `iconTexture` | string | 是 | 资源包内 UI 贴图路径 |
| `uvSize` | array | 否 | 默认 `[16, 16]` |

## 推荐命名

同一套盔甲建议使用相同的材质前缀：

```text
example:ruby_helmet
example:ruby_chestplate
example:ruby_leggings
example:ruby_boots
```

对应的 `materialKey` 建议统一为：

```text
example:ruby
```

这样同材质半格可以正常合并为完整护甲图标，不会出现中间分割线。

## 示例数据

参考 `examples/custom-armors.example.json`：

```json
{
  "itemName": "example:ruby_helmet",
  "defense": 2.5,
  "slot": "helmet",
  "materialKey": "example:ruby",
  "iconTexture": "textures/ui/detail_armor/ruby_armor",
  "uvSize": [16, 16]
}
```

## 迁移到项目配置

在细节盔甲项目中，开发者显式配置位于：

```text
detailab/common/customArmorConfig.py
```

把示例数据迁移为项目配置时，需要保持字段语义一致：

- `defense` 必须是真实护甲值，不建议为了显示而伪造
- `slot` 必须与装备实际槽位一致
- `materialKey` 必须稳定，同材质跨槽位保持一致
- `iconTexture` 必须是客户端资源包能访问的 UI 贴图路径

## 自动识别的边界

自动识别依赖物品基础信息和物品贴图路径。以下情况建议使用开发者显式配置：

- 盔甲没有暴露标准护甲值
- 盔甲槽位不是标准命名
- 同一材质不同部位无法通过名称推断
- 图标路径不是普通物品贴图
- 需要使用专门绘制的 HUD 图标

## 测试建议

- 四件套分别穿戴，确认护甲值总量正确
- 同材质半格相邻时，确认显示为完整图标
- 不同材质半格相邻时，确认左右半格分别显示
- 附魔左右半格不同时，确认光效不会串到另一侧
- 关闭自动识别后，确认未知盔甲按 `unknown` 设置回退
