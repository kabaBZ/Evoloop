# 自迭代项目基底（中文说明）

[English](README.md)

> **这是底座的 README，不是产品 README。** fork 之后，第一个 release 之前，
> 把本文件和英文 [`README.md`](README.md) 换成项目自己的 README。
> fork 步骤都在 [`docs/bootstrap.md`](docs/bootstrap.md)，换掉不会丢东西。
> Agent 如果还能看到这段提示，说明 bootstrap 没做完。

这是一个**给“由 Agent 写代码、Review、合并、发版”的项目用的底座**，不是一个应用。
它把 magpie 那种“三条 `/loop` 自动维护一个仓库”的运作方式抽出来，去掉产品细节，
保留真正约束 Agent 的东西：路由表、规则、错题本、社区闸门、发布闸门。

## 三条循环

| 循环 | 入口 | 干什么 | 人保留什么 |
| --- | --- | --- | --- |
| 1 | GitHub + 社区聊天 | 分诊 issue、逐行审 PR 并真跑、修复→发版→回复 | 安全闸门、产品方向 |
| 2 | 社交媒体 | 先用便宜模型过滤“这是不是真的反馈”，再决定修不修、回不回 | 公开语气、公关危机 |
| 3 | 代码库本身 | 每晚复盘当天提交、写教训、清理、提议黑名单 | 黑名单的最终决定 |

每个循环只守一个入口，各有自己的 checkpoint，互不抢活。**先跑一条，跑稳了再加下一条。**

## 目录

```
CLAUDE.md                  只 import 两个文件
AGENTS.md                  路由表：干什么活之前读哪份文档
LESSONS.md                 错题本：规则 + 原因 + 证据 + Seen N×
CHANGELOG.md               产品更新日志模板（每个版本一条）
LICENSE                    默认 MIT；要换宽松协议就改这里
SECURITY.md                安全问题走私密渠道，别开公开 issue
CONTRIBUTING.md            人类贡献者开口前先看这个
docs/
  BASE-CHANGELOG.md        底座自己的版本和变更，fork 回并时看这个
  bootstrap.md             开 fork 的清单，所有 {{占位符}}
  project-brief.md         产品是什么、给谁用、拒绝做什么
  decisions.md             已经拍板的决定和审美，不许被顺手推翻
  code-standards.md        改动规则、测试规则、验收标准、提交规则
  contributors.md          社区 PR 先查害、再评审；署名；黑名单只提议
  release.md               发版闸门：绿灯才打 tag，tag 不等于 release
  environments.md          真机复现矩阵、沙箱、凭据规则
  agents-memory.md         本地记忆和仓库文档怎么分工
  subsystems/              子系统文档的写法和模板
  standards/               语言/UI 附加规则（go.md、gui.md 是示例）
  loops/                   三条循环的参考提示词（按项目改，不要照抄）
  extraction-map.md        magpie 文件 → 本仓库的对照表
```

## 怎么用

1. `grep -rn '{{' .`，把 `docs/bootstrap.md` 表里的占位符全部填掉。
2. 写 `docs/project-brief.md`：产品边界、质量线、支持范围。
3. 把已经拍板的决定写进 `docs/decisions.md`，尤其是“看起来像 bug 但故意的”那些。
4. 按 `docs/environments.md` 准备真机/沙箱。**没有真机，Agent 的“已修复”就是猜的。**
5. 先只开 loop 1 的分诊和回复；稳了再加修复、评审、发版；最后加 loop 3、loop 2。

## LESSONS.md 为什么重要

这是整套东西的记忆体：

- 每晚复盘当天提交，只问三个问题：**什么又被修了一遍？什么本不该合并？什么验证太薄？**
- 每条教训写成：规则 → 为什么 → 证据（issue/commit/日期）→ `Seen N×`。
- `Seen` 数的是**不同的天**，同一天看到三次只算一次。
- 同一条被三天看到，就升级进 `docs/code-standards.md`，从错题本里删掉，
  在 “Moved to the code standards” 留一行。
- 也记 “Done right”：做对了的示范，供下一个 Agent 学。
- 每个 Agent 改代码前必须先读它。

没有这个文件，Agent 会反复踩同一个坑；有了它，坑才会变成规则。

## 几条不能动的底线

- 没在用户的实际环境里复现过，不许说“修好了”。
- 没跑的检查，必须写“没跑”，不许省略。
- 修一个 bug，必须 grep 同类兄弟一起修。
- Agent 自己的改动在 lint 和全量测试通过后直接进 `main`，PR 只留给社区贡献者；
  社区 PR 只合并审过的那一个 head，评审期间再推就重审。
- tag 必须等那次提交的 CI 全绿；被下一次 push 取消的 run 不算绿。
- 项目默认值永远不覆盖用户自己设的值。
- 读失败 = 未知，不是空；守卫要 fail closed。
- 已经写进 `decisions.md` 的决定，不许顺手推翻。
- 社区 PR 先查害，再谈质量。
- 黑名单：Agent 只提议、只给证据，人拍板。

## 和 magpie 的关系

- 保留：`CLAUDE.md` 两个 import 的结构、`AGENTS.md` 路由表、`LESSONS.md` 的
  复盘/计数/升级机制、六条验收标准、社区 PR 的查害和署名规则、发布闸门。
- 通用化：语言、平台、命令、供应商、发布方式全部变成占位符；Go/GUI 细节放进
  `docs/standards/` 当示例，不当普适规则。
- 没抄：magpie 的产品架构、provider 表、子系统页、把 release 转发到第二个仓库，
  以及三个 loop 的原始提示词照抄成可直接运行的提示词。原文已逐字记录在
  [`docs/loops/originals.md`](docs/loops/originals.md)；这三份 loop 文档是按它
  通用化的版本，是**参考设计，不是能直接跑的提示词**；每个项目要按自己的命令、
  入口和 checkpoint 存放位置改写，只保留安全底线。

细节和逐文件对照见 [`docs/extraction-map.md`](docs/extraction-map.md)。

底座自身的文件：[English README](README.md)、[底座更新日志](docs/BASE-CHANGELOG.md)、
[CHANGELOG.md](CHANGELOG.md)、[LICENSE](LICENSE)、[SECURITY.md](SECURITY.md)、
[CONTRIBUTING.md](CONTRIBUTING.md)。