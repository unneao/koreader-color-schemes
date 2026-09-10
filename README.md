# KOReader Color Schemes

一个 KOReader 阅读配色插件:在阅读菜单里一键切换**字体颜色 + 背景颜色**,无需手写 CSS、无需修改 KOReader 源码。

> 面向 EPUB / FB2 / TXT / HTML 等由 crengine 排版的可重排书籍;PDF、扫描件等固定版式不受影响。

## 特性

- 一键切换,单选列表,自动互斥,不用记 CSS。
- 配色跨书记忆,全局生效。
- 覆盖正文与标题、链接等所有文字颜色,并统一页面背景。
- 纯 Lua 用户插件,放进 `plugins/` 目录即可,免编译、免打补丁。

## 内置配色

| 名称 | 背景色 | 字体色 | 说明 |
| --- | --- | --- | --- |
| 默认 | — | — | 不染色,跟随书籍原样 |
| 米黄 | `#F9F2E2` | `#4A4030` | 暖色纸感,白天护眼 |
| 豆绿 | `#E2ECD9` | `#3D4B33` | 淡绿色,白天护眼 |
| 浅蓝 | `#E8EDF4` | `#39434E` | 冷调浅色 |
| 蓝灰 | `#14171C` | `#98A6B4` | 夜间,低蓝光 |
| 深灰 | `#1E1E22` | `#A3A3A5` | 夜间,深灰底 |
| 琥珀 | `#241A0D` | `#C9AC7C` | 夜间,暖琥珀低蓝光 |
| 纯黑 | `#000000` | `#8C8C8C` | 夜间,OLED 省电 |

## 安装


1. 下载本仓库,找到 `color_schemes.koplugin` 文件夹。
2. 放到 KOReader 数据目录下的 `plugins/` 里,没有 `plugins/` 就新建。
3. 完全退出并重启 KOReader。


## 使用

打开任意 EPUB → 顶部菜单 → 切到 **“排版 / Aa”** → 页面底部找到 **“Color schemes: 当前配色”** → 点进去直接单选。



## 原理

插件作为阅读器插件运行,在 `ReaderStyleTweak:getCssText()` 外面包一层,把当前配色的 CSS 追加到每次排版输出的末尾:

```css
html, body {
    color: <字体色> !important;
    background-color: <背景色> !important;
}
* {
    color: inherit !important;
    background-color: transparent !important;
    background-image: !important;
}
```

这样任何字体/排版设置变化触发的重新排版,配色都会自动保留,且无需改动 KOReader 任何源码。

## 自定义

编辑 `color_schemes.koplugin/main.lua` 顶部的 `SCHEMES` 表即可增删/改色:

```lua
local SCHEMES = {
    { id = "default", title = _("默认"), bg = nil, fg = nil },
    { id = "my_scheme", title = _("我的配色"), bg = "#F5F0E6", fg = "#333333" },
    -- ...
}
```

- `id` 需唯一,改完重启 KOReader。
- 列表顺序即 `SCHEMES` 的顺序。

## 已知限制

- 仅对 crengine 排版的**可重排格式**生效(EPUB/FB2/TXT/HTML…);PDF/DjVu/漫画等固定版式不受影响。
- 为获得统一背景,会清除书籍自带的块级底色(如代码块高亮),链接颜色也会统一为正文色。
- 若“插件管理”里被禁用,或 KOReader 开启了 `plugins_disable_external`,插件不会加载。

## 卸载

删除 `koreader/plugins/color_schemes.koplugin/` 文件夹并重启即可。

## 目录结构

```
color_schemes.koplugin/
├── main.lua      # 插件逻辑与配色表
└── _meta.lua     # 插件名称与描述
```

## License

建议以 AGPL-3.0 发布(与 KOReader 一致)。请在使用前自行添加 `LICENSE` 文件。

## 致谢与免责声明

- 配色参考了常见阅读 App 的观感,自行调校,与任何商业产品无关联。
- KOReader 是独立开源项目,本项目为第三方插件,与 KOReader 官方无隶属关系。
