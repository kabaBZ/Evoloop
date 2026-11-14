<p align="center">
  <img src="docs/assets/evoloop-banner.zh-CN.svg" alt="EvoLoop" width="100%">
</p>

<p align="center">
  <a href="README.en.md">English</a> · <strong>简体中文</strong>
</p>

<p align="center">
  <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-22D3EE?style=flat-square">
  <img alt="Autonomous loops: 3" src="https://img.shields.io/badge/Autonomous%20loops-3-818CF8?style=flat-square">
  <img alt="Maintained by agents" src="https://img.shields.io/badge/Maintained%20by-agents-C084FC?style=flat-square">
</p>

---

## EvoLoop 是什么

EvoLoop 是一套面向 **「由 Agent 编写、审查、合并、发布」** 的项目的工程底座。它不是一个应用，而是围绕 Agent 运行的操作系统：一份路由表、一组不可协商的硬规则、一本会把重复错误升级为硬规则的错题本、社区与发布闸门，以及三条长期运行的自治循环。

> 它要解决的不是「Agent 能不能写代码」，而是 **「Agent 写错过的代码，会不会在同一个坑里再摔一次」**。

三条原则贯穿整个仓库：

- **人保留方向与品味，Agent 承担执行与验证。** 产品边界、审美、安全与黑名单由人拍板，其余交给循环。
- **每次返工都沉淀为规则**，而不是一句「下次注意」。
- **「已修复」只在报告者的真实环境里成立。** 编译通过不算，隔壁平台的交叉构建也不算。

## 核心机制

### 一、三条自治循环

| 循环 | 入口 | 职责 | 人保留 |
| --- | --- | --- | --- |
| **Loop 1** | GitHub 与社区聊天 | 分诊 issue、评审并合并社区 PR、发布、回复 | 安全闸门、产品方向 |
| **Loop 2** | 社交媒体 | 先用低成本模型过滤信号，再复现真实反馈、修复、发布、回复 | 公开语气、公关危机 |
| **Loop 3** | 代码库本身 | 每晚复盘当天提交、整理教训、清理、提议黑名单 | 黑名单的最终决定 |

每个循环只守一个入口，并维护自己的 checkpoint，彼此不共享队列。同一件事如果落在两个循环的视野里，归入口所有者的循环处置，另一个只做链接。

**先跑一条，跑稳了再加下一条。** 三条同时开，往往意味着没有一条有清晰的入口和责任。

### 二、会进化的错题本

```
今天的提交
    │
    ▼  Loop 3 夜间复盘（只问三件事）
    │   · 什么又被修了一遍？
    │   · 什么本不该合并？
    │   · 什么验证得太薄？
    ▼
LESSONS.md ── 每条教训 = 规则 + 原因 + 证据 + Seen N×
    │
    │  同一条教训在 3 个不同日期出现
    ▼
docs/code-standards.md ── 升级为硬规则，之后的每次评审都检查
```

`LESSONS.md` 不是日记，而是项目的长期记忆。**每个 Agent 改代码前必须先读它**；读之前犯过的错，读之后就不再是新的。没有这个文件，Agent 会在同一个坑里反复交学费；有了它，坑才会变成墙。

### 三、以真实环境为准的验证闸门

- 在报告者的 **操作系统、构建版本、Agent/客户端和真实数据** 上复现，才算修好。
- **没跑的检查必须写「没跑」**，不许省略，也不许含糊成「应该没问题」。
- 修一个 bug 时，**grep 出所有同类兄弟一起修**，而不是只修样本。
- 读取、解析或哈希失败时，结果记为 **「未知」而不是「空」**，守卫要 fail closed。
- 项目默认值 **永远不覆盖** 用户已经显式设置的值。

## 快速开始

EvoLoop 以**模板**形式发布。把它 fork 或复制成自己的仓库后，先完成一次性的 bootstrap，再让循环接管日常维护。

1. **填占位符。** `grep -rn '{{' .` 列出全部 `{{TOKEN}}`，按 [`docs/bootstrap.md`](docs/bootstrap.md) 的表格逐一替换。
2. **写产品边界。** 在 [`docs/project-brief.md`](docs/project-brief.md) 里写清楚产品是什么、给谁用、明确拒绝什么。Agent 无法替你决定从未被告知的产品问题。
3. **记录已拍板的决定。** 把「讨论过并选了 X」以及「看起来像 bug 但就是故意的」写进 [`docs/decisions.md`](docs/decisions.md)。
4. **准备真实环境。** 按 [`docs/environments.md`](docs/environments.md) 建立复现矩阵。**没有真机，Agent 的「已修复」就是猜的。**
5. **接上 CI 与分支保护。** 把 lint、快速测试、全量测试接进 CI；给 Agent 身份配置 **actor 级**的直接推送豁免，而不是整条分支的通用豁免。
6. **按顺序开循环。** 第 1 周只开 Loop 1 的分诊与回复；第 2 周加上直接推送、评审与发布；第 3 周开 Loop 3；第 4 周再开 Loop 2。

## 仓库结构

```
CLAUDE.md                     唯一入口，只 import AGENTS.md 和 LESSONS.md
AGENTS.md                     路由表：干什么活之前读哪份文档 + 硬性不变量
LESSONS.md                    错题本：规则、原因、证据、Seen N×
README.md / README.en.md      中文主 README / 英文 README
CHANGELOG.md                  产品更新日志模板（每个版本一条）
LICENSE                       默认 MIT
SECURITY.md                   安全问题走私密渠道
CONTRIBUTING.md               人类贡献者开 PR 前必读
docs/
  bootstrap.md                一次性开仓清单，所有 {{占位符}} 的契约
  project-brief.md            产品、用户、非目标、质量线
  decisions.md                已拍板的决定与审美，不许顺手推翻
  code-standards.md           改动、测试、验收、评审、提交规则
  contributors.md             社区 PR 的查害、署名与黑名单规则
  release.md                  发布与 tag 闸门
  environments.md             真机复现矩阵、沙箱与凭据规则
  agents-memory.md            本地记忆与仓库文档的分工
  extraction-map.md           magpie 源文件 → 本仓库的对照表
  assets/                     项目图标与横幅
  subsystems/                 子系统文档的写法、模板与索引
  standards/                  语言/UI 附加规则（Go、GUI 为示例）
  loops/                      三条循环的参考设计与原始提示词记录
.github/                      PR 模板与 issue 模板
```

## 文档导航

| 你想… | 先读 |
| --- | --- |
| 改代码 | [`LESSONS.md`](LESSONS.md)，再读 [`docs/code-standards.md`](docs/code-standards.md) |
| 改一个有文档的子系统 | [`docs/subsystems/README.md`](docs/subsystems/README.md) 与该子系统页 |
| 评审或合并社区 PR | [`docs/contributors.md`](docs/contributors.md)，**先查害再评审** |
| 发版或打 tag | [`docs/release.md`](docs/release.md) 与验收标准 |
| 定默认值、行为、布局或依赖 | [`docs/decisions.md`](docs/decisions.md)、[`docs/project-brief.md`](docs/project-brief.md) |
| 复现用户报告 | [`docs/environments.md`](docs/environments.md) |
| 开或运行自治循环 | [`docs/loops/README.md`](docs/loops/README.md) |
| 写或升级一条教训 | [`LESSONS.md`](LESSONS.md) |
| 开 fork、填占位符、启动循环 | [`docs/bootstrap.md`](docs/bootstrap.md) |

## 不可让步的底线

这些是让「自治维护」保持安全的部分。不要在 fork 时删掉它们，除非你换上了更强的机制：

- **验证必须是真的。** 用户的原案例，在用户实际使用的界面上，在真实环境里。
- **没跑的检查必须列为「没跑」。** 不许省略，不许暗示它已经通过。
- **修一个 bug 要覆盖所有同类。** 发布前先 grep 同样的模式。
- **Agent 的改动在 lint 与全量测试通过后直接进 `main`；PR 留给社区贡献者。** 社区 PR 只合并审过的那一个 head，评审期间再推就重审。
- **tag 必须等该提交的 CI 全绿。** 被后续 push 取消的 run 不算通过。
- **默认值永远不覆盖用户设置。** 写值之前，先读完所有读取该设置的地方。
- **读取失败 = 未知，不是空。** 守卫必须 fail closed。
- **已记录的决定不会被顺手推翻。**
- **社区贡献先查害，再谈质量。**
- **黑名单由 Agent 提议、由人拍板。**

## 与 magpie 的关系

EvoLoop 的运作模型来自 [yetone/magpie](https://github.com/yetone/magpie)：三条长期运行的 Agent 循环，分别对应 GitHub/社区、社交媒体和代码库本身，再加一本把重复错误升级为硬规则的 `LESSONS.md`。

本仓库保留这套机制，剥离产品细节：语言、平台、命令、供应商和发布方式全部参数化；Go、GUI 等具体规则作为**示例附加条款**放在 `docs/standards/`；三条 loop 的原始提示词逐字记录在 [`docs/loops/originals.md`](docs/loops/originals.md)，仓库里的 loop 文档是**参考设计，不是可直接运行的提示词**。逐文件对照见 [`docs/extraction-map.md`](docs/extraction-map.md)。

## 参与贡献

- **社区贡献者：** 先读 [`CONTRIBUTING.md`](CONTRIBUTING.md) 与 [`docs/contributors.md`](docs/contributors.md)。每个 PR 先过查害，再看质量；使用 [PR 模板](.github/pull_request_template.md) 写明 **你跑了什么、没跑什么**。空的验证记录足以关闭一个 PR。
- **Coding Agent：** 从 [`AGENTS.md`](AGENTS.md) 和 [`LESSONS.md`](LESSONS.md) 开始。Agent 的改动直接落在 `main`，但推送前必须跑 lint 与全量测试，推送后盯住该提交的 CI，红了就向前修，不许 force-push。
- **安全问题：** 通过 [`SECURITY.md`](SECURITY.md) 私密报告，不要开公开 issue。

## 关于本仓库

EvoLoop 目前以**底座模板**的形式发布：本 README 描述的是框架本身。仓库中的 `{{TOKEN}}` 占位符是刻意的，填写契约在 [`docs/bootstrap.md`](docs/bootstrap.md)；fork 成具体产品后，应使用产品自己的 README 替换本文件。底座自身的版本变更见 [`docs/BASE-CHANGELOG.md`](docs/BASE-CHANGELOG.md)。项目图标源文件：[`evoloop-logo.svg`](docs/assets/evoloop-logo.svg)。

## 许可证

[MIT](LICENSE)