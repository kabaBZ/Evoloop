# The original loop prompts

These are the three `/loop` prompts the magpie maintainer ran, supplied to this
base on 2026-11-13 as a screenshot and transcribed here **verbatim**. They are
the source, not the runtime prompts for a fork. The three loop pages
([1](1-github-and-community.md), [2](2-social-feedback.md),
[3](3-nightly-review.md)) generalize them for another project and add the
safety rails this base requires.

Read this page when you want to know what the original asked for. Read the loop
pages when you want to run a loop.

## Loop 1 — GitHub, pull requests, Discord

```text
/loop 盯一下 GitHub issues，仔细分析，看看哪些是值得修复和实现的，修复或者实现后记得发版和回复，如果不值得修复或者实现也要回复，如果是中转站的广告商，就统一回复：感谢关注，日后会出相关合作方案，敬请期待。而不能一股脑地把第三方中转站直接加到我们产品里面。也要盯一下GitHub的PR，然后你要认真的去review PR的每行代码、每个逻辑，你要把代码拉下来实际跑一下，验证一下是否没有 bug。因为我怕有的人提的PR，是无脑用Coding Agent乱写的，自己都没测过。所以你要看一下他这个PR，是否有问题，确保一切都没有问题，且实际验证过之后也没有问题的话，你才能够去合并，否则你就去回复一下这个PR或者关掉 PR。
另外还要监控一下 Magpie Discord Server 里用户的反馈，实时的加入讨论，如果有价值的反馈和建议，就去做修复跟添加新的功能。如果值得进一步讨论，那就继续讨论。记得查看 Magpie Discord Server 里所有的 channel，一个都不能漏掉！记得在 Discord 里要真实地 at 人，而不是只叫名字，不然对方收不到你的消息！
```

What it asks for, in plain terms: triage every GitHub issue and reply either
way; fix and release the ones worth fixing; give relay and reseller ads one
canned reply and never integrate them; review every line and every branch of a
PR, pull it and actually run it, because the author may be an untested coding
agent; merge only after that verification, otherwise reply or close; read every
Discord channel, join the discussion, and use a real mention when replying to a
person.

## Loop 2 — Social feedback

```text
/loop 盯一下推特上关于 magpie 的反馈，如果是有价值的反馈，或者是真实可复现的 bug，那么就去做实现或者修复。完毕后记得上线，上线后记得回复推特

你应该先用便宜的决策模型比如 jev 去判断推文是不是与 magpie 相关，是不是 magpie 的反馈，如果不是直接忽略不要把内容转发给你，不然太浪费 Token 了
```

What it asks for: watch posts about the project; implement or fix valuable and
reproducible feedback; ship it, then reply to the post; put a cheap decision
model in front of the main model so unrelated posts never reach it.

## Loop 3 — Cleanup, lessons, block list

```text
/loop 现在 magpie 完全是自动化更改代码和合并社区里的 PR，你要定时清理一下屎山代码和恶意代码防止项目变得难以维护以至于变废，并总结出经验教训，比如哪些是不应该合并的哪些是验证不缜密的等等放到经验中防止以后的 agent 犯同样的错误，以及要把老是提低质量代码的人拉入黑名单
```

What it asks for: periodically clean up the pile of messy code and remove
malicious code before the project becomes unmaintainable; write down what was
learned — which PRs should not have merged, which verification was too thin —
so the next agent does not repeat it; block people who keep submitting
low-quality code.

## Where this base deliberately differs

The originals assume a trusted agent with broad authority. A general base
cannot assume that, so three points are changed on purpose. Keep the change
unless the fork has a better gate.

| Original | This base | Why |
| --- | --- | --- |
| Loop 2 releases the fix itself (`上线后回复`) | Loop 2 opens a PR; loop 1 reviews, merges and releases; loop 2 replies with the version | One release authority. Loop 2 has no merge or tag token, so a social post can never publish unreviewed code. The reply waits for the release, which is the honest order. |
| Loop 3 blocks low-quality authors directly | Loop 3 appends evidence to the [block-list proposals table](../contributors.md#block-list-proposals); the maintainer decides | A block is irreversible and personal. An agent may collect evidence; it may not punish. |
| Loop 3 runs "periodically" (`定时`) | Loop 3 runs nightly over a watermark window | A fixed window makes the review idempotent and auditable. The cadence is still a project choice; the watermark is what makes it safe. |
| Loop 3 cleans up "shit mountain" code | Only behavior-preserving cleanup at night; a real refactor is a normal PR with a goal, tests and review | An unattended refactor is the fastest way to produce a huge diff no one reviewed. Refactors deserve a reviewer and a rollback plan, not a nightly agent. |
| The canned reply is Chinese text | `{{CANNED_PARTNERSHIP_REPLY}}`, defaulted to English | Placeholders keep the base language-neutral. Keep the meaning: one reply, no third-party relay in the product. |
| "Review every line, pull it, run it" | Same intent, made procedural: run at the reviewed head, record what ran, merge with a head lock | The original states the standard; the base adds the evidence that proves it was met. |

Loop 1's specifics are kept without reduction: reply even when declining, one
canned reply for relay ads, never integrate them, real `@` mentions, every
channel. The loop page adds checkpoints, idempotency, release discipline and a
`Never` list; it does not weaken the original.

## What the originals do not contain

They are short because the maintainer was writing for an agent that already had
the repository rules in context. They do not define: the checkpoint store, the
release procedure, what counts as verified, how a lesson is recorded or
promoted, how to handle secrets, or how to report. Those are in
[`../code-standards.md`](../code-standards.md),
[`../release.md`](../release.md), [`../../LESSONS.md`](../../LESSONS.md) and
the loop pages. Do not mistake the original's brevity for the whole process.
