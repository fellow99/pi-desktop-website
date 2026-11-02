# AGENTS.md — pi-desktop-website（Pi Desktop 产品官网）

> 本文件是本工程（`pi-desktop-website`）的 AI Agent 工作规范与导航入口。
> 本文档是网站的设计文档（`README.md`）＋ GitHub Actions 部署的收敛导读。

---

## 一、项目定位

**Pi Desktop Editions 的产品官网（落地页）** —— `pi-desktop` / `pi-desktop-hos` / `pi-desktop-ark` /
`pi-desktop-android` 四个工程的共同门面。**纯静态、零依赖、零外部请求**（无框架、无构建步骤、无字体/CDN/统计），
必须能在 `file://` 下完整离线渲染，并经 GitHub Actions 部署到 GitHub Pages
（线上：<https://fellow99.github.io/pi-desktop-website/>）。

## 二、关键文件

| 文件 | 作用 |
|---|---|
| `index.html` | 英文页（`lang="en"`） |
| `index_zh.html` | 简体中文页（`lang="zh-CN"`），与 `index.html` **结构 1:1** |
| `style.css` | 共享样式表（全部主题走 CSS 自定义属性） |
| `script.js` | 共享主题切换逻辑（语言中立，`data-label-*` 驱动） |
| `images/logo.svg` | 产品 Logo（源自父工程 `../assets/logo.svg`） |
| `images/*.png` | 真实产品截图（浅色 / 深色主题） |
| `.github/workflows/pages.yml` | GitHub Pages 部署 |

## 三、硬性约束（必须永远成立）

- **零依赖约束**：不得引入外部字体 / CDN / 统计 / 任何网络请求；纯 HTML / CSS / 原生 JS。
- **双页纪律**：任何文案 / 结构变更必须 EN 与 ZH **两页同步**（结构 1:1）。
- **文案准确度护栏**：版本号、补丁数（分版计：desktop **0** · hos **2** · ark/HarmonyOS **6** · android **6**）、
  平台覆盖、能力表述必须与四个产品 README 及父工程一致。**改版本号时，本站文案 + 四个产品 README 必须一起改。**
- **品牌纪律**：配色跟产品 Logo 的绿；措辞「让 Pi agent harness 在桌面安家」等口径不得随意改。

## 四、常用命令

无构建步骤（纯静态）。本地验证：直接用浏览器打开 `index.html` / `index_zh.html` 检查离线渲染与 EN/ZH 双页一致。

## 五、CodeGraph 代码地图规范

> 本工程已建立 CodeGraph 语义索引（文件少、以 `script.js`/`style.css` 为主）。

- 索引位置：`.codegraph/`（不入库，已 `.gitignore`）。
- **自动更新**：git `post-commit` 钩子（`.git/modules/pi-desktop-website/hooks/post-commit`）在每次提交后自动 `codegraph sync`，非阻塞、不破坏提交。
- 常用命令：`codegraph status` / `codegraph query <符号>`。
- 改动后查不到，先 `codegraph sync` 再查。

## 六、提交纪律

- Conventional Commits（`docs`/`feat`/`fix`/`chore` 等）。
- **严禁** `git add -A` / `git add .`：只暂存本会话改动的文件。
- 检查 `git status`，避免把 Agent 产生的临时文件卷进无关提交。
- 严禁提交密钥 / Token / 口令（本工程本不应出现）。

## 七、安全红线

- 严禁 `as any` / `@ts-ignore` / `@ts-expect-error` 压制类型错误。
- 修复 Bug 只做最小改动，禁止顺手重构；禁止臆测未读取的代码。
