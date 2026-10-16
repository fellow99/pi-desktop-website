# 101 — 版本更新：0.12.1（version）测试用例与执行结果

> **Module:** version ｜ **Status:** Implementing ｜ **Last Updated:** 2026-10-15/16
> **需求编号:** 101-version-0.12.1 ｜ **上游基线:** pi v1.2.0 · pi-web v0.12.1
> **规格依据:** [`spec.md`](./spec.md) ｜ **任务:** [`tasks.md`](./tasks.md)
> **原始取证:** `logs/20261015-1/`（本轮日志目录，`.gitignore` 已忽略，不入库）

图例：✅ 通过 ｜ ❌ 失败 ｜ ⬜ 未执行（含阻塞原因） ｜ N/A 本工程不适用

> 站点是纯静态站点、无构建步骤：本节「测试」= 静态事实一致性 + HTML 良构 + 离线自检。
> 所有用例的结果已按 2026-10-15/16 轮次实测**回填**（证据在 `logs/20261015-1/`）；未执行项（`html-validate`/`tidy` 良构校验、claim 全文巡检、提交与打 tag）逐行写明原因。命令一律在仓库根 `pi-desktop-website/` 下运行。

## A. 版本串与残留

| ID | 用例 | 方法（可复现命令） | 期望 | 结果 | 证据 |
|---|---|---|---|---|---|
| TC-01 | 追踪文件中**无旧版本串**残留 | `git grep -n -E "0\.11\.1\|1101\|v?1\.1\.0"`（仅扫被追踪文件） | 命中的每一处都能被解释为「历史行 / 规格正文」；`index.html`、`index_zh.html` **零命中**；`README.md` 仅 §10 历史行命中 | ✅ `T05-website-check.txt` §1 列出全部 `0.12.1` 命中；§2 的旧值残留**只在 `README.md` 的历史行**（`:47`/`:64`/`:90`/`:94`/`:95`/`:203`/`:224`，逐条可解释为历史/待办正文），两页零命中 | `T05-website-check.txt` |
| TC-02 | 两个落地页**零**旧版本串 | `git grep -n -E "0\.11\.1\|1101\|v?1\.1\.0" -- index.html index_zh.html` | 输出为空 | ✅ 两页五处落点全部为新值（§1），§2 残留清单中**无**两页行 | `T05-website-check.txt` |
| TC-03 | README 事实表已更新为新版本 | `git grep -n "0\.12\.1" -- README.md` + 读 L83 / L85 / L87 / L93 / L94 | `pi-desktop` 版本 `v0.12.1`、`pi-desktop-hos` `v0.12.1` / `versionCode 1201`、上游 `pi v1.2.0` + `pi-web v0.12.1` 均命中 | ✅ §1：`README.md:44`（当前版本行）、`:45`（上游基线）、`:84`（`pi-desktop` 版本）、`:86`（`pi-desktop-hos` + `1201`）、`:88`（上游基线）、`:89`（上游补丁分版口径）、`:90`/`:94`/`:95`。⚠️ 行号与用例原文的 L83/85/87/93/94 有漂移（回写时事实表增行） | `T05-website-check.txt` |
| TC-04 | `index.html` 五处版本行已更新 | `grep -nE "0\.12\.1\|1201" index.html` + 读 L92 / L94 / L96 / L176 / L195 | L92 桌面 `v0.12.1`；L94 鸿蒙 `v0.12.1 · versionCode 1201`；L96 `pi v1.2.0 + pi-web v0.12.1`；L176/L195 状态行为 `v0.12.1` | ✅ 五处逐条命中（含 L94 的 `2 registered upstream patches` 分版口径） | `T05-website-check.txt` |
| TC-05 | `index_zh.html` 五处版本行已更新 | `grep -nE "0\.12\.1\|1201" index_zh.html` + 读 L92 / L94 / L96 / L176 / L195 | 与 TC-04 同位置同值（中文表述） | ✅ 五处逐条命中，位置与 EN 页完全同号（`:92/:94/:96/:176/:195`） | `T05-website-check.txt` |
| TC-06 | README 版本史行已追加、历史行未改 | 读 `README.md` §10；`git diff README.md` 的 §10 hunk | 新增一行 `0.12.1`（日期 **2026-10-15/16**，与 §2「分发状态」及真机取证日期一致）；v1.0–v1.2 三行逐字未改（无删除行） | ✅ `README.md:229` 为新增的 `v1.3 \| 2026-10-16` 回写行；v1.0–v1.2 行未被改写（§2 的旧值命中仅来自历史行文本） | `README.md` §10、`T05-website-check.txt` §2 |
| TC-07 | 事实表 `来源` 列可验证（无悬空来源） | 对事实表每个版本相关行的 `来源` 文件逐一打开核对其值 | 每行来源文件确实含该事实；无「来源文件里没有该值」的行 | ⬜ 未完成：① 版本相关行（`package.json` / `app.json5` / `upstream.lock.json`）已由 §6 交叉核对；② 「发布产物」「分发与签名」两行的来源（`git tag` / GitHub Release 页）在**未打 tag** 前不可核实（本轮未提交未打 tag，见 TC-11 / TC-14）；③ 本轮未落盘逐行来源核对的取证件 | `T05-website-check.txt` §6（缺逐行核对取证件） |

## B. EN / ZH 一致性

| ID | 用例 | 方法（可复现命令） | 期望 | 结果 | 证据 |
|---|---|---|---|---|---|
| TC-08 | 两页版本串**逐处一致** | 抽取两页中的版本 token 后对比：`grep -oE "v0\.[0-9]+\.[0-9]+\|versionCode [0-9]+" index.html \| sort > /tmp/en.txt`，`index_zh.html` 同法 → `diff /tmp/en.txt /tmp/zh.txt` | 两份 token 列表**完全相同**（仅语言不同、值一致） | ✅ `T06-selfcheck.log` 的 EN/ZH 结构一致性 PASS，且两页 trust 行/状态行版本串同值（`v0.12.1` / `versionCode 1201` / `pi v1.2.0 + pi-web v0.12.1`）；§1 显示两页落点行号一一对应 | `T06-selfcheck.log`、`T05-website-check.txt` §1 |
| TC-09 | 两页标记骨架 **1:1** | 抽取两页 DOM 元素序列 / 节点数对比（`README.md` §10 v1.2 记载骨架 `204 = 204`） | 元素序列一致、节点数相等 | ✅ `T06-selfcheck.log`：`EN/ZH 结构一致性：PASS（h1 1=1｜cards 6=6｜li 18=18）`；§3 标签计数两页逐项相同（`blocks=9 li=20 h3=9 sup=2`） | `T06-selfcheck.log`、`T05-website-check.txt` §3 |
| TC-10 | 两页除版本外无结构漂移 | `git diff --stat` + 逐 hunk 审阅 | 两页仅版本相关行变化（L92/L94/L96/L176/L195 及必要口径行）；class / 结构 / 交互零变化 | ✅ §3 两页标签计数逐项相同、§1 改动集中于版本/口径行；⚠️ 本轮未落盘 `git diff --stat` 的逐 hunk 审阅取证件 | `T05-website-check.txt` §3 |

## C. README 事实表 vs 真实产物

| ID | 用例 | 方法（可复现命令） | 期望 | 结果 | 证据 |
|---|---|---|---|---|---|
| TC-11 | `pi-desktop` 版本与真实产物一致 | `grep -m1 '"version"' ../pi-desktop/package.json`；`git -C ../pi-desktop tag -l v0.12.1` | `0.12.1` 且 tag `v0.12.1` 存在（= 事实表 L83 / L93 的值） | ⬜ 阻塞：① 本仓自检读取 `../pi-desktop/package.json` **失败**（§6 报 `Cannot find module 'D:\pi-workspace\pi-desktop\package.json'`）→ 本仓无成功取证件（该值可由 `../pi-desktop/logs/20261015-1/T05c-typecheck.log` 的 `pi-desktop@0.12.1` 旁证）；② tag `v0.12.1` 本轮**未创建** → L93 的来源不可核实 | `T05-website-check.txt` §6（报错段） |
| TC-12 | `pi-desktop-hos` 版本 / `versionCode` 与真实产物一致 | `grep -nE 'versionName\|versionCode' ../pi-desktop-hos/AppScope/app.json5` | `versionName "0.12.1"`、`versionCode 1201`（= 事实表 L85 的值） | ✅ §6 交叉核对输出 `"versionName": "0.12.1"` / `"versionCode": 1201`（本仓自检成功项） | `T05-website-check.txt` §6 |
| TC-13 | 上游基线与真实锁文件一致 | 读 `../pi-desktop/upstream.lock.json`（pi / pi-web 的 tag） | `pi v1.2.0` + `pi-web v0.12.1`（= 事实表 L87 的值） | ✅ §6 输出 `pi tags = v1.2.0`、`pi-web tags = v0.12.1`；`../pi-desktop/upstream.lock.json` 与两个产品文档头为同值 | `T05-website-check.txt` §6 |
| TC-14 | 发布产物 / 分发行与真实发布一致 | 核对 `pi-desktop` GitHub Release 资产清单与 `pi-desktop-hos` 的 tag/Release 状态 | 事实表 L93 / L94 的「tag / 资产数 / 有无 Release」与真实一致 | ⬜ 未执行：本轮未提交、未推 tag、未产出 Release（无 tag `v0.12.1`、无资产清单可核） | `—` |

## D. HTML 良构与站点完整性

| ID | 用例 | 方法（可复现命令） | 期望 | 结果 | 证据 |
|---|---|---|---|---|---|
| TC-15 | `index.html` 良构 | `npx --yes html-validate index.html`，或 `tidy -errors -quiet index.html` | 零解析错误、零未闭合标签（仅允许既有风格的告警） | ⬜ 未执行：本轮未跑 `html-validate` / `tidy`；§5 的标签计数粗检给出 `li=20/18`、`p=36/18`（计数不配对，**不足以**判定良构）。间接支持：`T06-selfcheck.log`（该页 console 错误/异常 = 0、结构一致性 PASS） | `T06-selfcheck.log`（间接；缺良构校验取证件） |
| TC-16 | `index_zh.html` 良构 | 同 TC-15，目标换为 `index_zh.html` | 零解析错误 | ⬜ 未执行：同 TC-15（同一轮未落盘良构校验取证件；`li=20/18`、`p=36/18` 的计数不配对同样出现） | `T06-selfcheck.log`（间接；缺良构校验取证件） |
| TC-17 | GitHub Pages workflow **未被改动** | `git diff --stat -- .github/workflows/pages.yml`；`git diff -- .github/workflows/pages.yml` | 输出为空（零改动） | ✅ §7 显示 `.github/workflows/pages.yml` 仍是 2026-10-13 的 691 B 版本；本仓 `git status` 的改动清单只有 `README.md` / `index.html` / `index_zh.html`（+ 新增 `specs/`），不含该文件 | `T05-website-check.txt` §7 |
| TC-18 | 样式 / 脚本未被改动 | `git diff --stat -- style.css script.js` | 输出为空 | ✅ 本仓 `git status --short` 无 `style.css` / `script.js`；§4 复核两页只引同源 `script.js`（`index.html:286` / `index_zh.html:286`），无新增外链 | `T05-website-check.txt` §4 |
| TC-19 | `file://` 离线自检（无网络依赖） | 以 `file://` 打开两页 + headless 抓控制台/网络（`README.md` §9 口径） | 控制台零错误、零外部请求、仅同源文件请求 | ✅ `T06-selfcheck.log`：两页 `console 错误/异常 = 0`；请求总数 `index.html 7` / `index_zh.html 6`，**非 `file://` 外部请求 = 0** | `T06-selfcheck.log` |

## E. 定性 claim 的取证（每条 claim 必须有已验证来源）

| ID | 用例 | 方法（可复现命令） | 期望 | 结果 | 证据 |
|---|---|---|---|---|---|
| TC-20 | 桌面**验证范围** claim 有 `0.12.1` 证据 | 读 `../pi-desktop/docs/构建验证记录.md`（0.12.1 轮）与两页 L92 / L176 表述对照 | 页面所写验证面（仅 Windows / 含 macOS·Linux）与 `0.12.1` 记录**完全一致**；若记录扩大则页面已同步放宽 | ✅ `../pi-desktop/docs/构建验证记录.md` 轮次 4 已含 0.12.1 轮（`smoke-desktop` 10 PASS / 0 FAIL、打包态 `probe-renderer` 7 PASS / 0 FAIL、基本功能 e2e 9 PASS / 0 FAIL；macOS / Linux **未验**），与两页 L92/L176「Windows 打包态已验收；macOS & Linux 待验证」一致 | `../pi-desktop/docs/构建验证记录.md`（轮次 4）、`../pi-desktop/logs/20261015-1/TEST_REPORT.md` |
| TC-21 | 鸿蒙**真机** claim 有 `0.12.1` 证据 | 读 `../pi-desktop-hos/docs/真机验证记录.md`（0.12.1 轮）与两页 L94 / L191 表述对照 | 设备型号 / API 等级 / 「已构建、已签名、真机验证」结论以 `0.12.1` 记录为准；无照抄旧 `HAD-W32` / API 26 的过期表述 | ✅ `../pi-desktop-hos/docs/真机验证记录.md` 轮次 7 已含 0.12.1 轮（设备 `3QC0226526001227`、`HAD-W32` / API 26、`versionName 0.12.1` / `versionCode 1201`、14 项设备断言 PASS / 0 FAIL），与两页 L94/L191 一致（设备/API 本轮未变，属**确认**而非照抄） | `../pi-desktop-hos/docs/真机验证记录.md`（轮次 7）、`../pi-desktop-hos/logs/20261015-1/T08-basic.log` |
| TC-22 | 每条定性 claim 可回溯到来源（无悬空 claim） | 逐条列出两页各定性句 → 在 `README.md` §2 护栏/事实表中找到对应 `来源` | 每条 claim 均能落到「产品 README / 源码 / 验证记录」；无凭空断言 | ⬜ 部分：产品面 claim（验证范围 / 真机 / 补丁分版）已能落到两个产品仓库的 README、`patches/README.md` 与验证记录；但「已打 tag `v0.12.1`」类 claim 的核实阻塞于 TC-11 / TC-14（本轮未打 tag），且本轮未落盘「逐条 claim → 来源」的对照取证件 | `T05-website-check.txt`（缺逐条对照取证件） |
| TC-23 | 无「未取证能力」被正面宣传 | `git grep -niE "auto.?update\|code.?sign\|notari\|store\|official" -- index.html index_zh.html`（人工复核命中语义） | 仅出现「未签名 / 无自动更新 / 非官方」等**否定/澄清**表述；无正面宣传 | ⬜ 未执行：本轮未落盘该 grep 的取证件；可得的旁证是 `T06-selfcheck.log` 抽取的两页全文（trust 行 / 结果徽章），其中签名相关表述均为否定式（「安装包未签名」） | `—`（缺 grep 取证件） |

## F. 提交与发布

| ID | 用例 | 方法（可复现命令） | 期望 | 结果 | 证据 |
|---|---|---|---|---|---|
| TC-24 | 提交符合 Conventional Commits + `site` scope | `git log --format='%s' <base>..HEAD` | 每条均匹配 `^(feat\|fix\|docs\|style\|refactor\|perf\|test\|build\|ci\|chore\|revert)\(site\):` | ⬜ 待提交后核验（本轮未提交） | `—` |
| TC-25 | tag `v0.12.1` 已创建且指向本需求最后提交 | `git tag -l v0.12.1`；`git describe --tags`；对比 `git rev-parse HEAD` | tag 存在且指向本需求最后一次提交 | ⬜ 待打 tag（本轮未提交、未打 tag） | `—` |
| TC-26 | 未改动任何分支；未动父仓库 | `git branch --show-current`（应为 `main`）+ 分支列表前后快照；核对未写 `../`（`pi-workspace` 父目录） | 分支列表与执行前一致；父仓库零改动 | ⬜ 待提交后核验（无前后分支快照；本轮未提交、未动父仓库，但无用例要求的 `T07-tag` 取证件） | `—` |

## G. 不适用（N/A）

| 项 | 为什么 N/A |
|---|---|
| 编译 / 单元测试 / 运行时测试 | 站点为纯静态、无构建步骤、零依赖；无编译器/运行时可测（改由 A–D 的静态一致性用例覆盖）。 |
| 素材（截图 / Logo）重构 | 本需求不改素材（spec §8）。 |
| CTA / 下载入口变更 | 属待办 ⬜，本需求排除（spec §2 Out of scope）。 |
| `pi-desktop` / `pi-desktop-hos` 的构建、真机与发布 | 那是两个姊妹工程的 `101-version-0.12.1`；本站点只消费其结论（TC-20/21 引用其记录）。 |

---

## 结果汇总（2026-10-15/16 轮次已回填）

| 区段 | 用例数 | ✅ | ❌ | ⬜ | N/A |
|---|---|---|---|---|---|
| A 版本串与残留 | 7 | 6 | 0 | 1 | 0 |
| B EN / ZH 一致性 | 3 | 3 | 0 | 0 | 0 |
| C README 事实表 vs 真实产物 | 4 | 2 | 0 | 2 | 0 |
| D HTML 良构与站点完整性 | 5 | 3 | 0 | 2 | 0 |
| E 定性 claim 的取证 | 4 | 2 | 0 | 2 | 0 |
| F 提交与发布 | 3 | 0 | 0 | 3 | 0 |
| **合计** | **26** | **16** | **0** | **10** | **0** |

> 站点无编译器 / 运行时可测（见 §G 的 N/A 项）。剩余 ⬜ 为两类：① 须先提交 / 打 tag 才能核实的项（TC-07 部分、TC-11、TC-14、TC-22、TC-24…TC-26）；② 本轮未落盘取证件、无法自证的项（TC-15、TC-16、TC-23）。逐行原因见结果列。
