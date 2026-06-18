# Aurio 1.0.0 — Claude 工作指南

> Aurio 是匿名化的虚构品牌；本项目为个人设计作品集 demo，与任何真实公司无关。

## 项目概述

**Aurio 助听器伴侣 App 的 iOS PWA 交互原型。**
- 单文件实现：所有 HTML、CSS、JS 都在 `index.html`（约 5700+ 行）
- 设计 token 集中在 `aurio-tokens.css`
- 完整设计系统文档见 `design-system.md`
- 目标设备：iPhone（390×844），模拟手机外壳展示

---

## 文件结构

| 文件 | 作用 |
|------|------|
| `index.html` | 全部 HTML + CSS + JS，单一真相来源 |
| `aurio-tokens.css` | CSS 自定义属性（颜色、间距、圆角、阴影、动画） |
| `design-system.md` | 组件清单、排版规范、动画参数（开发参考） |
| `tokens.json` | 设计 token 的 JSON 导出（备用） |

---

## 导航流程（CRITICAL — 改界面前必读）

### 层级结构

```
.phone（手机外壳容器）
├── .status（iOS 状态栏，固定顶部）
├── main.scroll × 3（主页面，同一时间只有一个 display:block）
│   ├── #volumePage     ← Tab 1：音量控制
│   ├── #noisePage      ← Tab 2：降噪模式
│   └── #mediaPage      ← Tab 3：媒体（流媒体/通话）
├── .tabbar（底部标签栏，始终可见）
├── .disconnect-overlay ← 蓝牙断连状态（z-index 叠加）
├── .bt-pair-overlay    ← 蓝牙配对弹窗（启动页「开始探索」后）
└── .splash-overlay     ← 启动页（最先显示，唯一入口「开始探索」）
```

### Tab 导航

Tab 点击 → 隐藏所有 `main.scroll` → 显示对应 `data-page` 的页面。  
Tab class `.active` 控制高亮状态。

### Overlay 显示规则

所有 overlay 默认 `display:none`，加 `.active` class 后显示。  
`disconnectOverlay` 用 `data-state="disconnected|connected"` 控制内部状态。

---

## 启动流程

```
启动页 splashOverlay（默认 .active）
  └── "开始探索" (#splashExplore) → 关闭 splash → 蓝牙配对 btPairOverlay → 进入主 Tab 区
```

> 亲友代调（家属远程协助）功能已移除；启动页仅保留「开始探索」单一入口。

## 页面内子状态

| 页面 | 子状态 |
|------|--------|
| `noisePage` | 4 个 mode 按钮切换 `data-mode`（off/comfort/strong/ultimate） |
| `mediaPage` | stream/call 双 tab，控制对应 panel 显示 |

---

## CSS 约定

### Class 命名前缀

| 前缀 | 对应区域 |
|------|----------|
| `bt-` | Bluetooth 配对相关（启动页配对流程） |

### Token 使用规则（来自 Figma 同步）

- 颜色必须引用 `aurio-tokens.css` 里的 CSS 变量，不写裸 hex 值
- 若 Figma 颜色在 token 里无精确匹配，标出来让设计师决定
- 已知 token 速查：`--blue-main #234d77`、`--warm-main #c89e72`、`--txt-dark #18212d`、`--txt-soft #6d7682`、`--color-error #FF4B3A`

---

## Figma → HTML 同步工作流

1. 在 Figma 选中 Frame，复制链接
2. 发链接给 Claude，说明目标 + "颜色用 aurio-tokens.css 变量"
3. Claude 通过 MCP 读取 Figma 数据，比对 token，更新 index.html

Figma MCP 已配置为项目级 SSE 服务器，重启 Claude Code 后自动可用。
