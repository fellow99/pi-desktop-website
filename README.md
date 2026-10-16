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
- **卖点（能力卡）**：100% 官方 Pi / Pi Web、**上游补丁分版计**（`pi-desktop` 桌面版 **0 条**补丁；`pi-desktop-hos` 鸿蒙版 **2 条**登记的上游补丁 —— `P203-1`「选择工作区目录」、`PFIX-1`「会话导出进程内回退」）· 本地优先、进程内宿主 ·
  桌面公民（托盘 / 原生通知 / 单实例 / 窗口状态记忆）· 与 `pi` CLI 共用同一套运行环境。
- **架构故事（How it works）**：进程内宿主 → 同源 UI → 官方数据面（`fetch('/api/*')` 上行 +
  SSE 下行），收尾徽章「No CORS · No custom protocol · No login page.」（与两页现文案一致；旧描述里的「· Zero upstream patches」已按 v1.3 的分版补丁口径删除，补丁口径改由 hero trust 行**分版**表述）。
- **当前版本**：`pi-desktop` **v0.12.1** · `pi-desktop-hos` **v0.12.1**（`versionCode` 1201）；
  两者共同的上游基线是 `pi` **v1.2.0** + `pi-web` **v0.12.1**。
  改版本号时，本站文案、下面的事实表、以及两个产品 README 必须一起改。
- **分发状态**：`pi-desktop` 已打 tag **`v0.12.1`**（`v0.11.1` 曾在 GitHub Release 发布 **9 个资产**：
  Windows `Setup.exe` + `full.nupkg` + `RELEASES`、win32-x64 / darwin-arm64 / linux-x64 三个 zip、
  macOS `arm64.dmg`、`amd64.deb`、`x86_64.rpm`，CI 与 Release 流水线当时均已跑绿）；
  `v0.12.1` 的 Release 由推送 tag 触发 `release.yml` 产出，**本轮只做到本地 commit + tag，未在 CI 侧复核**；
  `pi-desktop-hos` 已打 tag **`v0.12.1`**，但**无 Release**（HAP 目前只在本地产出）。
  ⚠️ 页面**尚未**因此新增下载入口 —— 见下方「页面 CTA 口径」与 §9 待办。


### 文案准确度护栏（必须永远成立）

- **每条 claim 都必须能回溯到产品 README / 源码 / 验证记录。** 今天已经落进页面的例子：
  「Windows 打包态端到端已验收、macOS / Linux 待验证」出自
  `pi-desktop/docs/构建验证记录.md` 轮次 1–3；「已构建、已签名、真机验证（HUAWEI MateBook Pro `HAD-W32`，HarmonyOS API 26）」
  出自 `pi-desktop-hos/docs/真机验证记录.md`；「必需补丁 0 条」出自两个工程的 `patches/` 与 README。
- **绝不虚构能力**：页面上不出现自动更新、代码签名、公证、商店分发、「官方产品」这类说法 ——
  两个壳都是**独立的、非官方的**开源工程，建立在 Pi 与 Pi Web 之上，
  **与上游作者无隶属关系、未获其背书**。
- **页面 CTA 口径**：`pi-desktop` 现在**已有** tag `v0.12.1`（`v0.11.1` 的 GitHub Release 有 9 资产），但
  页面 CTA **仍一律指向 GitHub 仓库**、**不出现"下载安装包"入口** —— 是否改成直接指向 Release
  下载页属**待办 ⬜**（见 §9），本文件只登记事实、不擅自改页面。`pi-desktop-hos` 有 tag 无 Release，
  其 CTA 只能指向仓库；trust 行只说"版本号是多少、验证到哪一步"。
- **未验证项按严格口径处理**：能力卡只写有证据的内容。已实测定论的项**可以正面写**
  （桌面：终端 / PTY ✅、原生通知 ✅）；仍未取证的项只按"已实现"陈述（桌面：系统托盘视觉呈现 ⬜）。
  `pi-desktop-hos` 已知不可用的项（终端 / PTY、无 `git`、无 `rg`/`fd`、插件仅本地源）
  **不在页面正面宣传**，只在需要区分桌面 / 鸿蒙时以「已知限制」的形式出现在平台卡的 bullet 里。
- **托盘与原生通知**：代码已实现（`pi-desktop/src/main/{tray,notifications}.ts`）并随包构建。
  **原生通知**已在轮次 2 用 P0-5 专项取证 —— 聚焦不弹 · 最小化弹出（延迟 **2054 ms**）·
  同会话 **10 s 冷却去重** · 冷却后可再弹 · 启动不补弹 = **5 PASS / 0 FAIL**
  （`pi-desktop/docs/桌面能力清单.md` §A#7 已 ✅）；系统**托盘的视觉呈现**仍不可编程判定
  （§A#8 ⬜ / O7），页面只按"已实现"陈述。
- 截图是**简体中文界面的真实产品截图**；英文页在 caption 里明确说明这一点，避免误导。
- 产品事实变化时：**先改产品 README 与 `package.json` / `app.json5`，再改本文件事实表，最后改两页文案。**

### 事实表（页面文案的唯一依据）

| 事实 | 值 | 来源 |
|---|---|---|
| `pi-desktop` 版本 | **v0.12.1** | `pi-desktop/package.json` |
| 桌面运行时 | Electron **43.7.9**（Node 24.21.0 / Chromium 150.0.7871.250 / ABI 148） | `pi-desktop/README.md`、`upstream.lock.json` |
| `pi-desktop-hos` 版本 | **v0.12.1** / `versionCode` **1201** | `pi-desktop-hos/AppScope/app.json5` |
| 鸿蒙运行时 | Electron-on-鸿蒙 **37.2.3**（Node 22.17.0 / ABI 138） | `pi-desktop-hos/README.md` |
| 上游基线 | `pi` **v1.2.0** + `pi-web` **v0.12.1** | `pi-desktop/upstream.lock.json`、两个工程的 README 文档头 |
| 上游补丁 | `pi-desktop` **0 条**；`pi-desktop-hos` **2 条**（`P203-1` 四主目录「选择工作区目录」、`PFIX-1` 会话导出进程内回退）——按被 patch 的上游 tag 分目录登记（`patches/v0.12.1/`），由构建脚本幂等应用、构建结束即回退，上游工作树保持 pristine | `pi-desktop-hos/patches/README.md`、`pi-desktop/specs/012-branding/spec.md` |
| 桌面验证状态 | **Windows 打包态端到端已验收**（0.12.1 轮次：`smoke-desktop` 10 PASS / 0 FAIL、打包态 `probe-renderer` 7 PASS / 0 FAIL、基本功能 e2e 9 PASS / 0 FAIL；0.11.1 轮次另有 TC-01…TC-16 全 PASS 与安装器副本 R1–R7 7 PASS / 0 FAIL）；**macOS / Linux 待验证** | `pi-desktop/logs/20261015-1/`、`pi-desktop/docs/构建验证记录.md` |
| 鸿蒙验证状态 | **已构建、已签名、已真机验证**（0.12.1 轮次 14 PASS / 0 FAIL：API KEY 落沙箱 `mode=600`、沙箱内工作区目录、一次真实对话与会话 JSONL 落盘、SWC 走内置 WASM 零下载）；设备 HUAWEI MateBook Pro `HAD-W32` / HarmonyOS **API 26** | `pi-desktop-hos/logs/20261015-1/`、`pi-desktop-hos/docs/真机验证记录.md` |
| 桌面已知不可用 | **无阻断项** —— 终端 / PTY 已在轮次 2 实测 **✅ 可用**（根因是**本机安装不完整的空壳 prebuilds**、触发上游 `prebuild.js` 误判，**不是**上游缺 `win32-x64` 产物；已修并加构建期守卫 `FR-010-027`）。仍为 ⬜ 的只有「系统托盘视觉呈现」 | `pi-desktop/docs/桌面能力清单.md` §A#1 / §A#8 |
| 鸿蒙已知不可用 | 终端 / PTY ❌ · `git` ❌ · `rg`/`fd` ❌ · 插件联网安装 ❌ | `pi-desktop-hos/README.md` |
| 发布产物 | `pi-desktop`：tag **`v0.12.1`**（`v0.11.1` 的 GitHub Release 有 9 个资产：Windows `Setup.exe` / `full.nupkg` / `RELEASES`、win32-x64 · darwin-arm64 · linux-x64 三个 zip、macOS `arm64.dmg`、`amd64.deb`、`x86_64.rpm`）；`pi-desktop-hos`：tag **`v0.12.1`**、**无 Release**（HAP 仅本地产出） | `git tag`、GitHub Release 页 `v0.11.1`、`pi-desktop-hos/README.md` |
| 分发与签名 | 桌面版走 **GitHub Release**（tag `v0.12.1`；`v0.11.1` 时 CI / Release 流水线跑绿）；鸿蒙版目前只在本地产出 HAP。两者 v1 **不签名、不公证、不静默更新** | `pi-desktop/docs/发布与更新.md` §7 |
| 托盘 / 原生通知 / 单实例 / 窗口状态记忆 | **原生通知 ✅ 已专项取证**（P0-5：聚焦不弹 · 最小化弹出延迟 **2054 ms** · 同会话 **10 s 冷却去重** · 冷却后可再弹 · 启动不补弹 = 5 PASS / 0 FAIL）；系统**托盘视觉呈现 ⬜ 未取证**（O7）；单实例与窗口状态记忆为代码已实现并随包构建 | `pi-desktop/src/main/{tray,notifications,window-state}.ts`、`pi-desktop/docs/桌面能力清单.md` §A#7 / §A#8 / §A#12 / §A#13 |
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

- [ ] **⏳ 页面下载入口（未决）**：`pi-desktop` 已有 tag `v0.12.1`（`v0.11.1` 的 GitHub Release 有 9 资产）——
      是否把 Hero 主 CTA 从「仓库」改为「下载」（指向 Release 页）需单独决策；**决定前不改**
      两个语言页的 CTA `href`（现状：一律指向 GitHub 仓库）。
- [ ] **限制句只能写"当前仍成立"的**：桌面卡旧句「终端面板不可用」已于 2026-10-13（PTY 实测 ✅）删除；
      `pi-desktop-hos` 卡的限制句（无终端 / PTY、无 `git`、无 `rg`/`fd` 二进制、插件仅本地源）**须在每次版本轮次对照
      `pi-desktop-hos/docs/鸿蒙环境能力清单-v<版本>.md` 复核后**才可保留（0.12.1 轮次已复核）。
      改动前先回查 §2 事实表。
- [ ] **补丁口径**：页面任何「零上游改动」类表述都必须**分版**写 —— `pi-desktop` 0 条补丁、
      `pi-desktop-hos` 有登记补丁（当前 2 条）。每次上游升级后回查 `pi-desktop-hos/patches/README.md`。
- [ ] **改文案？** `index.html` **和** `index_zh.html` 都要改（必要时同步 §2 的事实表）。
- [ ] **加区块？** 两个语言页 1:1 镜像标记结构 + 在 `style.css` 里加 token / 样式。
- [ ] **改主题或交互？** 保持 token 驱动 + 无 JS 兜底 + reduced-motion 分支。
- [ ] **换截图？** 必须是**真实产品截图**，且明暗两张成套；切换机制必须留在纯 CSS
      （`data-shot` 规则），**永不**用 JS 驱动。
- [ ] **版本号变了？** 先改产品 README 与 `package.json` / `app.json5`，再改本文件事实表，最后改两页文案。
- [ ] **离线自检**：`file://` 打开两个页面，控制台零错误、零网络请求。
- [ ] **目检**：1280 / 768 / 390 三个宽度 × 明暗两个主题。
- [ ] **不新增外部请求**：不引 CDN、不引 web font、不加统计脚本。

## 10. 变更记录

| 版本 | 日期 | 内容 |
|---|---|---|
| v1.0 | 2026-10-12 | 建档：纯静态站点设计文档（定位 / 事实表 / 结构 / 设计系统 / 交互 / 响应式 / 素材 / 部署 / 维护）。 |
| v1.1 | 2026-10-13 | 按 0.11.1 事实回写：§2 新增「分发状态」与「页面 CTA 口径」；事实表新增「发布产物」行，更正「桌面验证状态」（轮次 1–3）、「桌面已知不可用」（终端 / PTY ❌ → **✅ 可用**，并更正根因）、「分发与签名」、「托盘 / 原生通知」（通知 ✅ / 托盘 ⬜）；删除两个语言页平台卡中已失效的桌面 PTY 限制句；§2 护栏与 §9 维护清单同步更新。 |
| v1.2 | 2026-10-13 | 评审回写：更正**鸿蒙真机口径** —— 原文写 `HarmonyOS 6.1.0.135 / API 24`（实为**参考工程**的版本，本项目文档明确标注其属 🔶 沿用），真机为 `HUAWEI MateBook Pro` / `HAD-W32` / `HAD-W24 7.0.0.107` / **API 26**（源：`pi-desktop-hos/docs/真机验证记录.md`）；同步改两页 hero 脚注与平台卡共 4 处（EN/ZH 结构 1:1 不变）；§2 与 §9 的「改事实的先后顺序」统一。自检：`file://` + headless Chrome —— 控制台零错误/零异常、仅 6 个同源文件请求、零外部请求、EN/ZH 结构骨架逐节点一致（204 = 204）、1280·768·390 三宽度无横向溢出。 |
| v1.3 | 2026-10-16 | 按 0.12.1 事实回写：§2 版本行与「分发状态」、事实表 8 行（`pi-desktop` / `pi-desktop-hos` 版本、上游基线、上游补丁、桌面验证状态、鸿蒙验证状态、发布产物、分发与签名）、§2 页面 CTA 口径同步。**同时更正一处失实声明**：原「零上游改动 / zero upstream changes」对**鸿蒙版已不成立**——`pi-desktop-hos` 现有 2 条登记在案的上游补丁（`P203-1`、`PFIX-1`，2026-10-14/15 引入，晚于本文件 v1.2 的成文时间）；两页的 hero trust 行、能力卡、disclaimer、结果徽章与 meta description 一并改为「桌面版 0 条补丁 / 鸿蒙版 2 条登记补丁」的分版口径。 |
