# pi-desktop-website

> **Pi Desktop Editions** 的产品官网（落地页）—— [pi-desktop](../pi-desktop) 与
> [pi-desktop-hos](../pi-desktop-hos) 两个工程的共同门面。
>
> 预期线上地址：<https://fellow99.github.io/pi-desktop-website/>

本文档是网站的**设计文档**：定位、文案口径、页面结构、设计系统、交互、本地化与部署规则。

---

## 1. 概述

**纯静态、零依赖**站点 —— 纯 HTML / CSS / 原生 JS，无框架、无构建步骤、无外部请求
（字体 / CDN / 统计一律不引）。必须能在 `file://` 下完整离线渲染，并通过
[GitHub Actions](./.github/workflows/pages.yml) 部署到 GitHub Pages。

| 文件 | 作用 |
|---|---|
| `index.html` | 英文页（`lang="en"`） |
| `index_zh.html` | 简体中文页（`lang="zh-CN"`），与 `index.html` **结构 1:1** |
| `style.css` | 共享样式表 —— 全部主题走 CSS 自定义属性 |
| `script.js` | 共享主题切换逻辑（语言中立，由 `data-label-*` 驱动） |
| `images/logo.svg` | 产品 Logo（源自父工程 `../assets/logo.svg`） |
| `images/light.png` `images/dark.png` | **真实产品截图**，浅色 / 深色主题，2372×1530 |

> 站点结构沿用同型工程 [`dsh-desktop-website`](https://github.com/fellow99/dsh-desktop-website)
> 已验证的形态：同样的零依赖约束、同样的 EN/ZH 双页纪律、同样的纯 CSS 主题机制。
> 差异只在**配色**（跟产品 Logo 的绿）与**文案**（Pi 的产品事实）。

## 2. 定位与关键信息

- **是什么**：两个把 `Pi` agent harness 变成一等公民桌面应用的**独立开源封装工程**的营销落地页 ——
  Electron 桌面壳（Windows / macOS / Linux）与 HarmonyOS 版（2in1 / 平板）。两者都在应用内承载
  **官方、未修改的 Pi Web UI**（同源加载）。
- **标语**：EN *"The Pi agent harness, at home on the desktop."* ·
  ZH「让 Pi agent harness 在桌面安家」。
- **四平台覆盖是标题级差异点** —— Windows、Linux、macOS（Electron 版）+ HarmonyOS。
  它出现在：Hero 的 lede、lede 下的四个系统 chip（`ul.os-strip`）、两张平台卡、平台区声明。
- **卖点（能力卡）**：100% 官方 Pi / Pi Web、零上游改动 · 本地优先、进程内宿主 ·
  桌面公民（托盘 / 原生通知 / 单实例 / 窗口状态记忆）· 与 `pi` CLI 共用同一套运行环境。
- **架构故事（How it works）**：进程内宿主 → 同源 UI → 官方数据面（`fetch('/api/*')` 上行 +
  SSE 下行），收尾徽章「No CORS · No custom protocol · No login page · Zero upstream patches」。
- **当前版本**：`pi-desktop` **v0.11.1** · `pi-desktop-hos` **v0.11.1**（`versionCode` 1101）；
  两者共同的上游基线是 `pi` **v1.1.0** + `pi-web` **v0.11.1**。
  改版本号时，本站文案、下面的事实表、以及两个产品 README 必须一起改。

### 文案准确度护栏（必须永远成立）

- **每条claim都必须能回溯到产品 README / 源码 / 验证记录。** 今天已经落进页面的例子：
  「Windows 打包态端到端已验收、macOS / Linux 待验证」出自
  `pi-desktop/docs/构建验证记录.md` 轮次 1；「已构建、已签名、真机验证（HarmonyOS 6.1.0.135 / API 24）」
  出自 `pi-desktop-hos/README.md`；「必需补丁 0 条」出自两个工程的 `patches/` 与 README。
- **绝不虚构能力**：页面上不出现自动更新、代码签名、公证、商店分发、「官方产品」这类说法 ——
  两个壳都是**独立的、非官方的**开源工程，建立在 Pi 与 Pi Web 之上，
  **与上游作者无隶属关系、未获其背书**。
- **两个仓库当前都没有 git tag、没有 GitHub Release** ⇒ 页面上**不出现"下载安装包"**，
  所有 CTA 一律指向 GitHub 仓库；trust 行只说"仓库已公开、版本号是多少、验证到哪一步"。
- **未验证项按严格口径处理**：能力卡只写有证据的内容；`pi-desktop-hos` 与桌面版已知不可用的项
  （终端 / PTY、鸿蒙无 `git`、无 `rg`/`fd`、插件仅本地源）**不在页面正面宣传**，
  只在需要区分桌面 / 鸿蒙时以「已知限制」的形式出现在平台卡的 bullet 里。
- **托盘与原生通知**：代码已实现（`pi-desktop/src/main/{tray,notifications}.ts`），
  已在这一版随包构建，但**系统级呈现尚未专项取证**（`docs/桌面能力清单.md` §A#7 / §A#8 仍为 ⬜）。
  页面按「已实现」陈述，事实依据登记在本表下方；一旦专项取证有结论，回写此处与产品文档。
- 截图是**简体中文界面的真实产品截图**；英文页在 caption 里明确说明这一点，避免误导。
- 产品事实变化时：**先改产品 README，再改本站两个语言页，最后改本文件的事实表。**

### 事实表（页面文案的唯一依据）

| 事实 | 值 | 来源 |
|---|---|---|
| `pi-desktop` 版本 | **v0.11.1** | `pi-desktop/package.json` |
| 桌面运行时 | Electron **43.7.9**（Node 24.21.0 / Chromium 150.0.7871.250 / ABI 148） | `pi-desktop/README.md`、`upstream.lock.json` |
| `pi-desktop-hos` 版本 | **v0.11.1** / `versionCode` **1101** | `pi-desktop-hos/AppScope/app.json5` |
| 鸿蒙运行时 | Electron-on-鸿蒙 **37.2.3**（Node 22.17.0 / ABI 138） | `pi-desktop-hos/README.md` |
| 上游基线 | `pi` **v1.1.0** + `pi-web` **v0.11.1** | `upstream.lock.json` |
| 上游补丁 | **必需补丁 0 条**（品牌化只作用于本工程产物副本） | `pi-desktop/specs/012-branding/spec.md`、`pi-desktop-hos/patches/README.md` |
| 桌面验证状态 | **Windows 打包态端到端已验收**（TC 16 PASS / 0 FAIL）；**macOS / Linux 待验证** | `pi-desktop/docs/构建验证记录.md` 轮次 1 |
| 鸿蒙验证状态 | **已构建、已签名、已真机验证**（HarmonyOS 6.1.0.135 / API 24） | `pi-desktop-hos/docs/真机验证记录.md` |
| 桌面已知不可用 | 终端 / PTY ❌（缺 `win32-x64` 原生件，与 ABI 无关） | `pi-desktop/docs/桌面能力清单.md` §A#1 |
| 鸿蒙已知不可用 | 终端 / PTY ❌ · `git` ❌ · `rg`/`fd` ❌ · 插件联网安装 ❌ | `pi-desktop-hos/README.md` |
| 分发与签名 | GitHub 仓库分发；v1 **不签名、不公证、不静默更新** | `pi-desktop/docs/发布与更新.md` §7 |
| 托盘 / 原生通知 / 单实例 / 窗口状态记忆 | 代码已实现并随包构建；**系统级呈现 ⬜ 未专项取证**（页面按「已实现」陈述，不作"已验收"宣称） | `pi-desktop/src/main/{tray,notifications,window-state}.ts`、`pi-desktop/docs/桌面能力清单.md` §A#7 / §A#8 / §A#12 / §A#13 |
| 与 `pi` CLI 共用 `~/.pi/agent` | 默认共享；设置内可切 `<userData>/pi-agent`（切换后需重启） | `pi-desktop/README.md`、`pi-desktop/docs/桌面能力清单.md` §A#14 |
| 站点仓库 | <https://github.com/fellow99/pi-desktop-website> | `git remote -v` |

## 3. 页面结构

两个语言页共享同一套纵向结构（6 个 landmark 区块）：

1. **Nav**（`header.site-header`）：Logo + 后缀字 `Pi Desktop` · 锚点链接（功能 / 平台 / 原理）·
   胶囊形语言切换（`EN / 中文`）· 主题按钮。锚点链接 ≤ 760px 隐藏；品牌 / 语言 / 主题保留。
2. **Hero**（`section#top`）：左列文案 —— mono eyebrow 胶囊、`<h1>`、lede、
   `ul.os-strip`（Windows / macOS / Linux / HarmonyOS 四个 chip）、CTA 行
   （GitHub 主按钮 + 鸿蒙版按钮 + ghost「How it works」）、trust 行（版本与验证状态）；
   右列视觉 —— 带边框的、随主题切换的应用截图 + mono caption。
3. **Features**（`#features`）：标题 + lede + 4 张图标卡（桌面端 2×2，≤ 820px 单列）。
4. **Platforms**（`#platforms`）：两张版本卡（tag + 能力 bullet + 仓库链接），下接覆盖声明。
5. **How it works**（`#how`）：3 个带编号的步骤 + 箭头连接，结尾一个结果徽章。
6. **Footer**：品牌 + 一句话 + ©/许可行 + 上游工程仓库链接
   （Pi、Pi Web、Electron、harmony-electron 运行时，与 `fellow99` 系列仓库并列）。

EN / ZH **必须保持结构一致** —— 区块顺序、元素 class、交互行为永不分化；只有文案被翻译。

## 4. 设计系统

### 主题 token（`:root` / `html[data-theme="dark"]` 上的 CSS 自定义属性）

| Token | 浅色 | 深色 | 用途 |
|---|---|---|---|
| `--bg` | `#f5f7f6` | `#090d0b` | 页面底色 |
| `--surface` | `#ffffff` | `#111815` | 卡片 / 导航底色 |
| `--text` | `#14181a` | `#e9efec` | 正文 |
| `--muted` | `#5b6663` | `#98a5a0` | 次级文字 |
| `--hairline` | `#e2e9e6` | `#1f2a25` | 分隔线 / 卡片描边 |
| `--accent` | `#0a7d5b` | `#3ee8ab` | 链接、图标描边、chip 文本（两主题均满足 AA） |
| `--accent-soft` | `#e9f6f0` | `#12241c` | eyebrow 胶囊、ghost 按钮底色 |
| `--brand` | `#12db96` | `#12db96` | **产品 Logo 的原色**：chip 圆点、主按钮底色（配深色字） |

- 截图阴影 token `--shot-shadow` / `--shot-shadow-hover` 同样按主题切换。
- `--radius-lg: 14px`；`--ease: cubic-bezier(0.22, 0.61, 0.36, 1)`。
- **`--accent` 与 `--brand` 是两个角色，不要混用**：`--brand` 是品牌原色（低对比、只做色块与圆点），
  `--accent` 是为可读性调过的版本（承担所有"文字"与"描边"职责）。浅色主题下直接用 `#12db96` 写文字会不达 AA。
- **主题解析顺序**：`<head>` 内联脚本在首帧前依据 `localStorage['pi-theme']`，
  否则 `prefers-color-scheme` 设置 `data-theme`（无闪烁）→ CSS 渲染 token；
  另有一个 `@media (prefers-color-scheme: dark)` 的 `:root:not([data-theme])` 块覆盖无 JS 场景。

### 字体与视觉语言

- 字体：只用系统字体栈 —— 无衬线含 CJK（`PingFang SC`、`Microsoft YaHei`）；
  mono 用于 eyebrow、tag、trust 行与图注（CLI / 工具的签名感）。
- mono `$` 前缀区块小标题，例如 `$ why a native shell` / `$ 为什么要原生外壳`。
- **单一强调色**，克制的用法（实心主按钮、细分隔线、chip 圆点）—— 不用渐变卡片。
- Hero 背景装饰：淡淡的 radial 光晕 + 64px 网格，全部由 token 驱动。
- 截图按「应用窗口」呈现：1px 细描边 + `--radius-lg` + 分层静止阴影。**不画假窗体装饰**
  （截图本身就是真实运行画面）。

## 5. 交互

1. **主题切换** —— 纯图标按钮；浅色模式显示**月亮**、深色模式显示**太阳**（图标 = 动作目标）。
   `aria-label` 描述的是**动作**，由 `data-label-dark/light` 在运行时切换。手动选择持久化到
   `localStorage['pi-theme']`。
2. **Hero 截图随主题切换** —— 两张 `<img data-shot="light|dark">` **同时存在于 DOM**，
   由**纯 CSS** 决定当前主题显示哪一张（特异性高于 `.hero-shot img` 默认值），
   因此即使 JS 尚未执行也能显示正确截图。**永不**用 JS 切换。
3. **悬停微动效（截图）** —— 刻意缓慢柔软：`transform: translateY(-4px)` 历时 `0.5s`，
   `box-shadow` 绽放历时 `0.65s`（`--shot-shadow-hover`）；`prefers-reduced-motion` 下禁用。
4. **Hero 入场** —— 一次性错峰淡入 / 上浮（`riseIn`，`fill-mode: backwards`，
   因此后续的 hover transform 不会被覆盖）；页面其它地方不动画。

## 6. 响应式行为

- ≥ 921px 双列 Hero（文案 : 截图 = `1fr : 1.44fr`，容器 `max-width` 1380px，
  让截图在宽屏上呈现约 1.5× 尺度）；≤ 920px 单列。
- 能力卡 / 平台卡网格 ≤ 820px 转单列；锚点链接 ≤ 760px 隐藏；`.cta-row` 按钮
  ≤ 640px 变全宽堆叠、`.steps` 转纵向。
- **实测（`file://` + 同源 iframe 逐宽度测量，EN/ZH 双页 × 明暗双主题）**：1280 / 768 / 390
  三个宽度下 `scrollWidth === clientWidth`、无任何元素越界；
  1280 → Hero 双列（约 `476px : 685px`）+ 能力/平台 2 列 + 步骤 3 列 + 锚点可见；
  768 → Hero 单列 + 能力/平台单列 + 步骤仍 3 列 + 锚点**仍可见**（隐藏阈值是 760）；
  390 → 全部单列 + 锚点隐藏 + CTA 按钮纵向堆叠；控制台零消息。

## 7. 素材

| 文件 | 来源 | 处理 |
|---|---|---|
| `images/logo.svg` | 父工程 `../assets/logo.svg`（与 `pi-desktop/resources/brand/logo.svg` 同源） | 逐字节复制，不重绘 |
| `images/light.png` | `../assets/screenshots/pc/1.png` | 逐字节复制（真实运行截图，浅色，2372×1530） |
| `images/dark.png` | `../assets/screenshots/pc/2.png` | 逐字节复制（真实运行截图，深色，2372×1530） |

- 截图**不做重编码 / 不缩放**：原生 2372×1530 在 2× 屏上更锐利，单张约 110 KB 可接受；
  且 2372:1530 = 1.55:1 与 16:9 不同，强行改成 1280×720 会**变形**。
  HTML 的 `width` / `height` 属性写真实像素值，显示尺寸交给 CSS。

## 8. 开发与部署

```bash
# 本地预览（任何静态服务器都行；也可以直接双击打开 index.html）
npx serve .        # 然后访问 http://localhost:3000/index.html
```

部署由 GitHub Actions（[`.github/workflows/pages.yml`](./.github/workflows/pages.yml)）负责：
每次推送 `main`（以及手动 `workflow_dispatch`）上传仓库根目录并部署到 GitHub Pages。
需要一次性设置：**Settings → Pages → Source → "GitHub Actions"**。

首次部署后的预期地址：<https://fellow99.github.io/pi-desktop-website/>

## 9. 维护清单

- [ ] **改文案？** `index.html` **和** `index_zh.html` 都要改（必要时同步 §2 的事实表）。
- [ ] **加区块？** 两个语言页 1:1 镜像标记结构 + 在 `style.css` 里加 token / 样式。
- [ ] **改主题或交互？** 保持 token 驱动 + 无 JS 兜底 + reduced-motion 分支。
- [ ] **换截图？** 必须是**真实产品截图**，且明暗两张成套；切换机制必须留在纯 CSS
      （`data-shot` 规则），**永不**用 JS 驱动。
- [ ] **版本号变了？** 先改产品 README 与 `package.json` / `app.json5`，再改本文件事实表，最后改两页文案。
- [ ] **离线自检**：`file://` 打开两个页面，控制台零错误、零网络请求。
- [ ] **目检**：1280 / 768 / 390 三个宽度 × 明暗两个主题。
- [ ] **不新增外部请求**：不引 CDN、不引 web font、不加统计脚本。
