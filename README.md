# Detail Armor Bar Devkit

面向网易《我的世界》开发者的细节盔甲 HUD 适配资源与教程仓库。

本仓库只包含公开说明、贴图资源、示例配置数据和开发者教程，不包含细节盔甲模组的运行时代码。

## 适用对象

- 需要让自定义盔甲在细节盔甲 HUD 中正确显示的模组开发者
- 需要适配自定义盔甲材质、护甲值、槽位和图标的资源作者
- 需要了解 `unknown` 回退图标和自动识别逻辑边界的维护者

## 当前内容

- `docs/custom-armor-adapter.md`：开发者手动适配教程
- `docs/texture-guide.md`：HUD 图标贴图规范
- `docs/settings-and-fallback.md`：自动识别与 unknown 回退说明
- `examples/custom-armors.example.json`：自定义盔甲映射示例数据
- `resources/textures/ui/detail_armor/armor_bar.png`：护甲 HUD 图标参考贴图
- `resources/textures/ui/detail_armor/unknown_armor.png`：未知盔甲回退图标

## 适配优先级

细节盔甲 HUD 对自定义盔甲的显示来源按以下优先级处理：

1. 开发者显式配置
2. 自动识别物品基础信息和贴图路径
3. `unknown` 回退图标

开发者显式配置优先级最高，适合无法被标准物品接口正确识别的特殊盔甲。

## 快速适配流程

1. 为每个自定义盔甲物品确定 `itemName`。
2. 填写真实护甲值 `defense`。
3. 填写槽位 `slot`，可用值为 `helmet`、`chestplate`、`leggings`、`boots`。
4. 为同一套盔甲使用相同 `materialKey`。
5. 准备 16x16 或等比例 UI 图标，并填写资源包内 `iconTexture` 路径。
6. 按 `examples/custom-armors.example.json` 的结构整理数据，再迁移到项目中的适配配置。

## 不包含的内容

- 不包含模组 Python 运行时代码
- 不包含客户端 HUD 渲染逻辑
- 不包含服务端物品解析逻辑
- 不包含网易开发者后台工程文件

## 官方群

806836568
