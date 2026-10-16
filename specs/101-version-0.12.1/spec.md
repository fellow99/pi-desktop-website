# 101 — 版本更新：0.12.1（version）功能规格

> **Module:** version ｜ **Status:** Implementing ｜ **Last Updated:** 2026-10-15/16
> **上游基线:** pi v1.2.0（Branch_v1.2.0）· pi-web v0.12.1（Branch_v0.12.1）
> **需求编号:** 101-version-0.12.1 ｜ **需求名称:** 版本更新：0.12.1
> **Git 分支:** 不改动（在 `pi-desktop-website` 当前分支 `main` 上直接提交，见 §6）
> **规划依据:** `README.md` §2（定位 / 护栏 / 事实表）、§9 维护清单、§10 变更记录
> **姊妹规格:** `../pi-desktop-hos/specs/101-version-0.12.1/spec.md`（同型流程，本规格沿用其结构）；
> 桌面侧版本事实的出处为 `../pi-desktop`（`package.json` / `upstream.lock.json` / GitHub Release）
> **前序状态:** 站点当前发布态为 `v0.11.1`（见 `README.md` §2 与 §10 v1.2）

## 1. 目的

把**站点**（`pi-desktop-website`）从已发布的 `v0.11.1` 事实**同步到 `v0.12.1`**：

1. 当 `pi-desktop` 与 `pi-desktop-hos` **双双**在 `0.12.1` 通过各自测试并发布（打 tag `v0.12.1`）后，
   站点的版本事实才具备更新前提；
2. 更新 `README.md` 的事实表（版本 / `versionCode` / 上游基线 / 发布产物 / 分发与签名）与 §10 变更记录；
3. 更新两个落地页（`index.html`、`index_zh.html`）里**每一处**硬编码版本串；
4. **重新核实**（而非照抄）继承自 `v0.11.1` 的**定性 claim**：桌面「Windows 打包态端到端已验收；macOS /
   Linux 待验证」与鸿蒙「已构建、已签名、真机验证（HUAWEI MateBook Pro `HAD-W32` / API 26）」——
   以 `0.12.1` 的测试/真机记录为准，确认或更正；
5. 提交（Conventional Commits，`site` scope）+ 打 tag **`v0.12.1`**。

> 站点是纯静态站点、无构建步骤：本需求的「测试」即静态事实一致性 + HTML 良构 + 离线自检（见 `test-cases.md`），
> 不是编译或运行时测试。

## 2. 范围

**In scope**

- README 事实表（§2）版本相关行的更新 + `来源` 列校正。
- README §2「当前版本」「分发状态」条目与 §9 维护清单、§10 变更记录的同步。
- `index.html` / `index_zh.html` 全部硬编码版本串的更新（hero trust 行 L92/L94/L96、平台卡状态行 L176/L195）。
- 继承的**定性 claim** 的复检与按证据改写。
- EN / ZH 两页版本信息与结构的 1:1 对齐核验。
- 静态测试取证（`logs/20261015-1/`）+ 提交 + 打 tag `v0.12.1`。

**Out of scope**

- 站点 CTA 口径变更（是否新增「下载安装包」入口指向 Release）—— 属**待办 ⬜**（`README.md` §9），本需求**不改**。
- 站点结构 / 设计系统 / 交互 / 响应式 / 素材的任何改动。
- 姊妹工程（`pi-desktop` / `pi-desktop-hos`）的版本升级、测试与发布 —— 那是它们的 `101-version-0.12.1`；
  本站点**只消费**其结论，缺一即阻塞。
- 上游 `pi` / `pi-web` 的任何改动（只做 tag/分支引用）。
- 父工程 `pi-workspace`（子模块指针与父 README）——**明确不动**。
- 新建 `plan.md`（小工程，本次不产出方案文档，决策浓缩进 §5「关键决策」）。

## 3. 上游事实（本规格前提，已核实）

| # | 事实 | 出处 / 核实方式 |
|---|---|---|
| U1 | 站点当前发布的版本事实：`pi-desktop` **v0.11.1**、`pi-desktop-hos` **v0.11.1**（`versionCode` **1101**）；共同上游基线 `pi` **v1.1.0** + `pi-web` **v0.11.1**。 | `README.md` L44–L45、事实表 L83/L85/L87；页面 hero trust 行 `index.html` L92/L94/L96 |
| U2 | 待发布的新版本：`pi-desktop` **v0.12.1**、`pi-desktop-hos` **v0.12.1**（`versionCode` **1201**）；新上游基线 `pi` **v1.2.0** + `pi-web` **v0.12.1**。 | `../pi-desktop-hos/specs/101-version-0.12.1/spec.md` U1–U4、FR-ver-013、AC-15；桌面侧同型需求 |
| U3 | 站点仓库**尚无** `specs/` 目录、**无** `docs/` 目录；纯 HTML/CSS/JS，零依赖，GitHub Pages 由 `.github/workflows/pages.yml` 部署（push `main` / `workflow_dispatch` 上传仓库根目录）。 | `git ls-files`；读 `README.md` §1、§8 与 `.github/workflows/pages.yml` |
| U4 | 站点每条 claim 必须可回溯（护栏）：`README.md` §2「文案准确度护栏」要求 claim 回溯到产品 README / 源码 / 验证记录；「事实表（页面文案的唯一依据）」逐行给出 `来源`（如 `pi-desktop/package.json`、`pi-desktop-hos/AppScope/app.json5`、`upstream.lock.json`）。 | 读 `README.md` §2 L54–L97 |
| U5 | 页面里硬编码版本串的确切落点（EN 与 ZH **同构、同行号**）：`index.html` **L92**（桌面 hero trust）、**L94**（鸿蒙 hero trust + `versionCode`）、**L96**（上游基线行）、**L176**（桌面平台卡状态）、**L195**（鸿蒙平台卡状态）；`index_zh.html` 对应 **L92/L94/L96/L176/L195**。全部 `0.11.1` / `1101` / `v1.1.0` 出现点已穷举。 | `git grep -n -E "0\.11\.1\|1101\|1\.1\.0"` |
| U6 | 现存**定性 claim**（必须复检，不得照抄）：① 桌面「Windows 打包态端到端已验收；macOS & Linux 待验证」（hero L92 + 卡 L176，来源 `pi-desktop/docs/构建验证记录.md` 轮次 1–3）；② 鸿蒙「已构建、已签名、真机验证（HUAWEI MateBook Pro `HAD-W32` / HarmonyOS API 26）」（hero L94 + 卡 L191，来源 `pi-desktop-hos/docs/真机验证记录.md`）。 | 读 `README.md` 事实表 L89/L90 与两页 L92/L94/L176/L191 |
| U7 | 站点仓库最近的提交 scope 约定为 `feat(site):` / `fix(site):`（Conventional Commits）。 | `git log --oneline -4` |
| U8 | 本轮日志目录 `logs/20261015-1/`；`.gitignore` 第 2 行忽略 `logs`，故取证文件**不入库**。 | 读 `.gitignore` L1–L2 |
| U9 | 站点**被追踪**文件共 11 个，其中文本文件为 `README.md` / `index.html` / `index_zh.html` / `style.css` / `script.js` / `.github/workflows/pages.yml` / `.gitignore` / `LICENSE`（其余为 `images/*`）。 | `git ls-files` |

## 4. 需求条目

> 只追加不修订；MUST / MUST NOT / SHOULD / MAY。`tasks.md` 逐条回引。

### 4.1 发布门禁

- **FR-site-001**（MUST）：站点版本更新**只允许**在 `pi-desktop` 与 `pi-desktop-hos` **两者**均已在 `0.12.1`
  通过各自测试并发布（各自打 tag `v0.12.1`）之后进行；任一未通过/未发布，本站点**不得**落盘 0.12.1 版本串。
  本条为**硬门禁**，不可跳过、不可「先改后补」。
- **FR-site-002**（MUST）：动笔前必须核对两个姊妹工程的新版本事实（`pi-desktop` 的 `package.json` /
  `upstream.lock.json` / GitHub Release；`pi-desktop-hos` 的 `AppScope/app.json5` 与真机记录），
  以其**实际产物**为准，不得以本规格预填的数字为准。

### 4.2 README 事实表与版本史

- **FR-site-003**（MUST）：`README.md` §2 事实表的版本相关行必须全部更新为 `0.12.1` 事实：
  「`pi-desktop` 版本」（L83）、「`pi-desktop-hos` 版本 / `versionCode`」（L85，**`1201`**）、
  「上游基线」（L87，`pi` **v1.2.0** + `pi-web` **v0.12.1**）、「发布产物」（L93）、「分发与签名」（L94）。
- **FR-site-004**（MUST）：事实表每行的 `来源` 列必须同步校正，仍然指向产品仓库的**实际文件**
  （`pi-desktop/package.json`、`pi-desktop-hos/AppScope/app.json5`、`upstream.lock.json` 等），
  且不得出现「来源指向的文件里并没有该事实」的情形。
- **FR-site-005**（MUST）：`README.md` §2「当前版本」条目（L44–L46）与「分发状态」段落（L47–L51）
  必须同步；§9 维护清单里提到版本号/Release 的条目（L202–L204）必须与实际一致。
- **FR-site-006**（MUST）：`README.md` §10 变更记录必须**追加**一行登记本次 `0.12.1` 回写（日期 2026-10-15/16、
  版本号 +1），历史行（v1.0–v1.2）**保留不改**。
- **FR-site-007**（MUST NOT）：不得删除或篡改 §10 的历史行、也不得改写历史轮次中的旧版本号；
  历史值只应作为历史出现。

### 4.3 落地页

- **FR-site-008**（MUST）：`index.html` 与 `index_zh.html` 中**每一处**硬编码版本串必须更新为新值：
  hero trust 行（桌面 `v0.12.1`、鸿蒙 `v0.12.1` + `versionCode 1201`、上游 `pi v1.2.0 + pi-web v0.12.1`）
  与两个平台卡状态行（`v0.12.1` / `versionCode 1201`）。落点见 U5。
- **FR-site-009**（MUST）：EN / ZH 两页的版本信息必须**逐处一致**（同位置同值，仅语言不同）；
  两页的标记结构、元素 class、交互行为必须保持 **1:1**（`README.md` §3：永不分化）。
- **FR-site-010**（MUST NOT）：两页中不得残留任何旧版本串（`0.11.1` / `1101` / `v1.1.0`）；
  唯一允许的例外是 §10 历史行与规格文档正文（它们**在描述**本次变更）。

### 4.4 定性 claim 复检（本需求重点）

- **FR-site-011**（MUST）：对 U6 列出的**每一条**定性 claim，必须依据 `0.12.1` 轮的实测/真机证据**重新判定**，
  逐条给出「确认 / 更正 / 删除」结论；**禁止**未看新证据就直接沿用 `v0.11.1` 的旧措辞。
- **FR-site-012**（MUST）：桌面验证范围 claim 必须反映 `0.12.1` 的**实际验证面**：
  若本轮仍只验 Windows，保留「macOS / Linux 待验证」；若本轮补验了 macOS / Linux，则必须**放宽/更正**为已验范围。
- **FR-site-013**（MUST）：鸿蒙真机 claim 必须以 `0.12.1` 的**真机验证记录**为准（设备型号 / `versionCode` /
  API 等级以新记录为准）；若设备或 API 等级变化，必须更正，不得照抄 `HAD-W32` / API 26。
- **FR-site-014**（MUST NOT）：不得在页面上出现未经 `0.12.1` 证据支撑的能力或口径：
  自动更新、代码签名、公证、商店分发、「官方产品」一律不得出现；未取证项不得正面宣传。

### 4.5 校验、提交与发布

- **FR-site-015**（MUST）：收尾必须做「无残留旧版本串」与「EN/ZH 一致」两项静态核验（见 `test-cases.md`
  A/B 组），并保留取证到 `logs/20261015-1/`。
- **FR-site-016**（MUST）：HTML 必须保持良构（无未闭合标签 / 无解析错误），且 `style.css` / `script.js` /
  `.github/workflows/pages.yml` 不受本次改动影响（Pages 部署能力不变）。
- **FR-site-017**（MUST）：提交信息必须使用 **Conventional Commits** 前缀，scope 用 `site`
  （`feat(site):` / `fix(site):` / `docs(site):` 等），**不得使用其它前缀或其它 scope**。
- **FR-site-018**（MUST）：测试通过后必须打 tag **`v0.12.1`**，且**不改动任何 git 分支**。
- **FR-site-019**（MUST NOT）：不得改动父仓库 `pi-workspace`（子模块指针、父 README 一律不动）。

## 5. 关键决策

| # | 决策 | 依据 |
|---|---|---|
| D1 | **什么是「可发布事实」**：只有能回溯到产品 README / `package.json` / `app.json5` / 验证记录的 claim 才可写入站点；未经 `0.12.1` 测试的证据不构成事实。 | `README.md` §2 护栏（U4）；FR-site-001 |
| D2 | **发布门禁 = 两个姊妹工程都通过**：`pi-desktop` 与 `pi-desktop-hos` **缺一不可**。因版本号纪律要求「本站文案、事实表、两个产品 README 一起改」（`README.md` §2 L46）。 | FR-site-001；README §2 |
| D3 | **定性 claim 一律「重新核实，不复制」**：桌面验证范围与鸿蒙真机结论按 `0.12.1` 证据重写，允许结论变化（可能扩大验证面、可能仍待验证、设备/API 可能不同）。 | FR-site-011/012/013；U6 |
| D4 | **改动顺序**：先改产品 README 与 `package.json` / `app.json5`（在姊妹工程内），再改本站 `README.md` 事实表，最后改两页文案。 | `README.md` §2 L77、§9 L213 |
| D5 | **不扩大本需求边界**：仅改版本事实与必要口径；CTA / 下载入口决策仍属待办 ⬜，本需求不动。 | `README.md` §9 L202–L204 |
| D6 | **不产出 `plan.md`**：站点为小工程，决策浓缩进本节，任务拆解见 `tasks.md`。 | 本需求约定 |

## 6. 验收标准

| 编号 | 验收条件（必须全部满足） | 判定方式 |
|---|---|---|
| AC-01 | `pi-desktop` 与 `pi-desktop-hos` 均已在 `0.12.1` 通过测试并打 tag `v0.12.1`（门禁满足） | 姊妹仓库 `git tag -l v0.12.1` + 各自测试报告 |
| AC-02 | `README.md` 事实表版本行全部为 `0.12.1`（`pi-desktop` / `pi-desktop-hos` / `versionCode 1201` / 上游 `pi v1.2.0` + `pi-web v0.12.1`），`来源` 列已校正 | `git grep -n "0.12.1" README.md` + 读文件 |
| AC-03 | `README.md` §10 变更记录新增 `0.12.1` 回写行，历史行保留不变 | 读文件 |
| AC-04 | `index.html` 与 `index_zh.html` 均无 `0.11.1` / `1101` / `v1.1.0` 残留（L92/L94/L96/L176/L195 全部更新） | `git grep -n -E "0\.11\.1\|1101\|1\.1\.0"` |
| AC-05 | EN / ZH 两页版本信息逐处一致，且标记结构 1:1（骨架节点数一致） | 抽取版本串 diff + 骨架对比 |
| AC-06 | 定性 claim（桌面验证范围 / 鸿蒙真机）与 `0.12.1` 证据一致，逐条可引源 | 逐条引证 `logs/20261015-1/` 取证 |
| AC-07 | HTML 良构（无解析错误 / 未闭合标签） | 解析器 / tidy 输出 |
| AC-08 | `.github/workflows/pages.yml` 未被改动，Pages 部署能力不变；`file://` 离线自检控制台零错误、零外部请求 | `git diff` + 离线自检输出 |
| AC-09 | 本需求提交前缀全部为 Conventional Commits 且 scope 为 `site` | `git log` |
| AC-10 | tag `v0.12.1` 已创建并指向本需求最后一次提交 | `git tag -l` / `git describe` |

## 7. 约束与风险

| # | 约束 / 风险 | 处置 |
|---|---|---|
| C1 | **不改动 git 分支** | 仅在 `pi-desktop-website` 当前 `main` 上提交；不新建/切换/重命名分支；不打上游分支。 |
| C2 | 门禁未满足就抢跑（姊妹工程尚未 0.12.1） | 严格串行：T01 门禁未过则**整个需求阻塞**，不得落盘任何 0.12.1 串（FR-site-001）。 |
| C3 | 照抄旧定性 claim 导致失实 | 每条 claim 必须附 `0.12.1` 新证据（FR-site-011/012/013）；无证据即删除或降级为「已实现」。 |
| C4 | 版本串「多点呈现」易漏改 | 本规格已穷举落点（U5）；收尾用 `git grep` 全量复核（FR-site-015、AC-04）。 |
| C5 | EN / ZH 漂移 | 两页同构、同行号；改动后做逐处比对 + 骨架一致性核验（AC-05）。 |
| C6 | 事实表 `来源` 与实际文件不符 | `来源` 列每一行都要能打开对应文件验证（AC-02、TC-07）。 |
| C7 | 误改站点结构 / 部署 | 只改文案与事实表；`style.css` / `script.js` / `pages.yml` 零改动（AC-08）。 |
| C8 | 误动父仓库 | 全程不写 `pi-workspace` 父目录（FR-site-019）。 |

## 8. 不做（明确排除）

- 不新增/改动页面结构、设计系统、交互、响应式与素材。
- 不改 CTA / 下载入口（仍属待办 ⬜）。
- 不做姊妹工程的版本升级、测试与发布（只消费其结论）。
- 不改上游 `pi` / `pi-web`。
- 不新增构建步骤或依赖（站点保持纯静态、零依赖）。
- 不动父工程 `pi-workspace`。
- 不产出 `plan.md`。
