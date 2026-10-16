# 101 — 版本更新：0.12.1（version）任务拆解

> **Module:** version ｜ **Status:** Implementing ｜ **Last Updated:** 2026-10-15
> **需求编号:** 101-version-0.12.1 ｜ **上游基线:** pi v1.2.0 · pi-web v0.12.1
> **规格依据:** [`spec.md`](./spec.md)（无 `plan.md`，决策见 spec §5）
> **日志目录:** `logs/20261015-1/`（`.gitignore` 已忽略 `logs`，不入库）

## 依赖关系

```
T01（门禁：等两个姊妹工程 0.12.1 发布）
  │
T02（README 事实表 + 版本史行）
  │
T03（index.html） ─┬─ T05（EN/ZH 对齐 + 无旧版本串校验）
T04（index_zh.html）┘
  │
T06（提交） ─ T07（打 tag v0.12.1）
```

- **T01 是硬门禁**：`pi-desktop` 与 `pi-desktop-hos` **都**在 `0.12.1` 通过测试并打 tag `v0.12.1` 之前，T02 起一律不得开始（spec FR-site-001 / C2）。
- T03 与 T04 互不依赖，可并行，但都必须在 T05 之前完成（T05 是两页的联合校验）。
- T02 在 T05 之前完成；T05 的「事实表 vs 实际产物」核对需要 T02 的产物。
- T06 依赖 T02–T05 全部完成；T07 依赖 T06。

## 任务清单

### T01 — 门禁：等待并核对两个姊妹工程 0.12.1 发布

| 项 | 内容 |
|---|---|
| 回引 | FR-site-001、FR-site-002；AC-01 |
| 动作 | ① 在 `../pi-desktop` 与 `../pi-desktop-hos` 分别确认 `git tag -l v0.12.1` 存在且指向各自最后提交，测试报告（各自 `specs/101-version-0.12.1/` 的 AC 结论）全绿；② 记录两工程的新版本事实来源快照：`pi-desktop/package.json`（`version`）、`pi-desktop/upstream.lock.json`（pi / pi-web 基线）、`pi-desktop-hos/AppScope/app.json5`（`versionName` / `versionCode`）；③ 记录 `pi-desktop` GitHub Release 资产数（若有）与 `pi-desktop-hos` 是否有 tag/Release |
| 完成判据 | 两个 tag `v0.12.1` 均存在；两工程测试结论均为通过；三份来源文件的实际值已取到并落盘。**任一未满足 → 需求阻塞，停止。** |
| 产出 | `logs/20261015-1/T01-gate.txt`（含来源文件实际值） |

### T02 — 更新 README 事实表 + 版本史行

| 项 | 内容 |
|---|---|
| 回引 | FR-site-003、FR-site-004、FR-site-005、FR-site-006、FR-site-007；AC-02、AC-03 |
| 动作 | ① 把 `README.md` §2 事实表版本行改为 `0.12.1`：`pi-desktop` 版本（L83）、`pi-desktop-hos` 版本 / `versionCode` **1201**（L85）、上游基线 `pi` v1.2.0 + `pi-web` v0.12.1（L87）、发布产物（L93）、分发与签名（L94）；② 校正各行 `来源` 列（`pi-desktop/package.json`、`pi-desktop-hos/AppScope/app.json5`、`upstream.lock.json`、GitHub Release 页）；③ 同步 §2「当前版本」条目（L44–L46）与「分发状态」段落（L47–L51）；④ 同步 §9 维护清单中涉及版本号/Release 的条目（L202–L204）；⑤ 按 T01 证据**复核并改写** §2 事实表的「桌面验证状态」（L89）与「鸿蒙验证状态」（L90）两行（见 T05）；⑥ 在 §10 变更记录**追加**一行 `0.12.1` 回写（日期 2026-10-15，历史行不动） |
| 完成判据 | 事实表无 `0.11.1` / `1101` / `v1.1.0` 残留（历史区除外）；每行 `来源` 指向的文件确含该事实；§10 新增行存在、历史行逐字未改 |
| 产出 | `README.md`（改动）+ `logs/20261015-1/T02-readme.txt`（改动摘要 + `git diff --stat`） |

### T03 — 更新 `index.html`（EN）

| 项 | 内容 |
|---|---|
| 回引 | FR-site-008、FR-site-012、FR-site-013；AC-04 |
| 动作 | ① hero trust 三行：L92 桌面 → `v0.12.1` + 依 T05 复核后的验证范围；L94 鸿蒙 → `v0.12.1` · `versionCode 1201` + 依真机记录的口径；L96 上游 → `Built on pi v1.2.0 + pi-web v0.12.1`；② 平台卡状态行：L176 桌面 `Status: v0.12.1`、L195 鸿蒙 `Status: v0.12.1 · versionCode 1201`；③ 若 T05 结论要求，改写 L191 鸿蒙真机描述中的型号/API 与 L92/L176 桌面验证范围 |
| 完成判据 | 本文件内 `0.11.1` / `1101` / `v1.1.0` **零残留**；定性句与 T05 的证据结论一致；除上述行外无其它改动 |
| 产出 | `index.html`（改动）+ `logs/20261015-1/T03-index.txt`（`git diff` 片段） |

### T04 — 更新 `index_zh.html`（ZH）

| 项 | 内容 |
|---|---|
| 回引 | FR-site-008、FR-site-009、FR-site-012、FR-site-013；AC-04、AC-05 |
| 动作 | 与 T03 **逐处对应**（同位置、同行号）：L92 / L94 / L96 / L176 / L195 更新为新版本事实；L191 鸿蒙真机描述按 T05 结论同步 |
| 完成判据 | 本文件内 `0.11.1` / `1101` / `v1.1.0` **零残留**；与 T03 后的 `index.html` 逐处对应一致（仅语言差异） |
| 产出 | `index_zh.html`（改动）+ `logs/20261015-1/T04-index_zh.txt` |

### T05 — 校验：EN/ZH 对齐 + 无旧版本串 + 事实表 vs 真实产物

| 项 | 内容 |
|---|---|
| 回引 | FR-site-011、FR-site-012、FR-site-013、FR-site-015、FR-site-016；AC-04、AC-05、AC-06、AC-07、AC-08 |
| 动作 | ① `git grep -n -E "0\.11\.1\|1101\|v?1\.1\.0"` 全仓全量扫，逐条判定「已修 / 历史 / 规格正文」；② 抽取两页版本串并 diff（EN/ZH 逐处一致）；③ 对比两页标记骨架（节点数 / 元素序列）；④ 事实表 vs 真实产物核对（`pi-desktop/package.json`、`pi-desktop-hos/AppScope/app.json5`、`upstream.lock.json`）；⑤ HTML 良构校验（解析器/tidy）；⑥ `git diff --stat` 确认 `style.css` / `script.js` / `.github/workflows/pages.yml` 零改动；⑦ `file://` 打开两页离线自检（控制台零错误、零外部请求） |
| 完成判据 | 旧版本串仅剩「历史 / 规格正文」可解释项；EN/ZH 逐处一致且骨架节点数相等；事实表每行与来源文件相符；HTML 零解析错误；结构与部署文件零改动；离线自检零错误、零外部请求 |
| 产出 | `logs/20261015-1/T05-grep.txt`、`T05-parity.txt`、`T05-fact-vs-source.txt`、`T05-html-validate.txt`、`T05-offline.txt` |

### T06 — 提交

| 项 | 内容 |
|---|---|
| 回引 | FR-site-017；AC-09 |
| 动作 | 加载 `git-commit` 技能，按 Conventional Commits 提交，scope 用 `site`：版本事实回写建议 `feat(site): 站点版本事实更新至 0.12.1`；口径更正若独立成提交用 `fix(site): …`；文档类用 `docs(site): …`。禁止其它前缀/其它 scope |
| 完成判据 | `git log` 中本需求提交前缀与 scope 全部合规；工作树干净 |
| 产出 | commits（+ `logs/20261015-1/T06-commit.txt` 记录 `git log -1 --stat`） |

### T07 — 打 tag `v0.12.1`

| 项 | 内容 |
|---|---|
| 回引 | FR-site-018、FR-site-019；AC-10 |
| 动作 | 在站点当前分支 `main` 上 `git tag v0.12.1`（不改任何分支、不动父仓库 `pi-workspace`）；核对 tag 指向本需求最后一次提交 |
| 完成判据 | `git tag -l v0.12.1` 命中；`git describe --tags` 与该提交一致；`git branch` 列表与执行前一致 |
| 产出 | tag `v0.12.1`（+ `logs/20261015-1/T07-tag.txt`） |

## 里程碑

| 里程碑 | 含任务 | 判定 |
|---|---|---|
| M1 门禁就绪 | T01 | AC-01 绿；否则阻塞 |
| M2 事实回写 | T02–T04 | AC-02…AC-04 绿 |
| M3 一致性校验 | T05 | AC-05…AC-08 绿 |
| M4 交付完成 | T06–T07 | AC-09、AC-10 绿 |
