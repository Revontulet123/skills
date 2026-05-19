<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 面向真实工程师的 Skills

[English](./README.en.md)

[![skills.sh](https://skills.sh/b/Revontulet123/skills)](https://skills.sh/Revontulet123/skills)

这是我日常用于真实工程工作的 agent skills 集合。它们关注工程质量、反馈循环和可维护性，而不是把代码生成当成一次性的碰运气。

真实应用很难做。GSD、BMAD、Spec-Kit 这类方法会尝试接管流程来降低复杂度，但代价往往是你失去控制，流程中的问题也更难定位和修复。

这个仓库里的 skills 刻意保持小而清晰：容易改、容易组合，也不绑定特定模型。你可以直接使用，也可以按自己的项目习惯改造成更贴合团队的版本。

如果你想跟进上游技能的更新和作者新发布的内容，可以订阅 Matt Pocock 的 newsletter：

[订阅 Newsletter](https://www.aihero.dev/s/skills-newsletter)

## 快速开始

1. 运行 skills.sh 安装器：

```bash
npx skills@latest add Revontulet123/skills
```

2. 选择要安装的 skills，以及要安装到哪些 coding agents。请确保选择 `/setup-matt-pocock-skills`。

3. 在你的 agent 里运行 `/setup-matt-pocock-skills`。它会询问：
   - 你使用哪种 issue tracker：GitHub、Linear，或本地文件
   - 你在 triage ticket 时会使用哪些标签，`/triage` 会依赖这些标签
   - 生成的文档应该保存在哪里

4. 完成后就可以开始使用。

## 为什么需要这些 Skills

我把这些 skills 当作一组工程护栏，用来修复 Claude Code、Codex 和其他 coding agents 中常见的失败模式。

### 1. Agent 没有真正理解我要什么

> "No-one knows exactly what they want"
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**：软件开发里最常见的失败模式是对齐失败。你以为对方理解了需求，直到看到实现才发现它完全偏了。

AI 时代也是一样。你和 agent 之间天然存在沟通缺口。解决办法是先做一次 grilling session：让 agent 在动手前追问细节、挑战假设、逼近真正的问题边界。

**对应 skills**：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md)：适合非代码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md)：在 `/grill-me` 的基础上加入工程文档能力

这两个是最常用的 skills。每次你准备让 agent 修改系统、设计功能或处理复杂任务前，都应该先用它们做需求对齐。

### 2. Agent 说得太啰嗦

> With a ubiquitous language, conversations among developers and expressions of the code are all derived from the same domain model.
>
> Eric Evans, [Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题**：项目初期，开发者和领域专家经常不在同一套语言体系里。Agent 进入项目时也一样，它不知道哪些词是项目里的约定用语，于是只能用很长的解释去描述一个本该有名字的概念。

**解决办法**：建立共享语言。它是一份帮助 agent 理解项目术语、业务概念和架构决策的文档。

<details>
<summary>示例</summary>

这是来自 `course-video-manager` 仓库的一个 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) 示例。对比下面两种说法：

- **之前**：当课程分区中的一节课被变成“真实文件系统里的内容”时会出现问题
- **之后**：materialization cascade 存在问题

后一种表达更短，也更适合在一次又一次会话中复用。

</details>

[`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 已经内置了这套做法。它不仅会追问需求，还会帮助你沉淀共享语言，并把难解释的决策写进 ADR。

> [!TIP]
> 共享语言的收益不只是减少啰嗦：
>
> - 变量、函数和文件命名会更一致
> - Agent 更容易在代码库里导航
> - Agent 思考时消耗的 token 更少，因为它可以使用更精确的项目语言

### 3. 代码跑不起来

> "Always take small, deliberate steps. The rate of feedback is your speed limit. Never take on a task that’s too big."
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**：就算你和 agent 已经对齐，它仍然可能产出不能运行、不可验证或不稳定的代码。

这通常不是单纯的“模型不够强”，而是反馈循环太弱。没有类型检查、浏览器验证、自动化测试或可复现的调试路径，agent 就是在盲写。

**解决办法**：建立反馈循环。自动化测试尤其关键，红绿重构可以让 agent 先写一个失败测试，再让实现把测试跑绿，最后再整理设计。

相关 skills：

- [`/tdd`](./skills/engineering/tdd/SKILL.md)：把 red-green-refactor 流程插入任何项目
- [`/diagnose`](./skills/engineering/diagnose/SKILL.md)：用可复现、可观测、可回归的方式处理 bug 和性能问题

### 4. 我们做出了难以维护的代码泥球

> "Invest in the design of the system _every day_."
>
> Kent Beck, [Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "The best modules are deep. They allow a lot of functionality to be accessed through a simple interface."
>
> John Ousterhout, [A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题**：Agent 可以显著加快写代码的速度，也会同步加快系统复杂度累积的速度。很多由 agent 参与构建的应用很快变得难以理解、难以修改、难以扩展。

**解决办法**：把代码设计当成 AI-assisted development 的核心部分，而不是后续补救。

这些 skills 会从不同层面帮助你保持设计质量：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md)：在写 PRD 前追问会触碰哪些模块
- [`/zoom-out`](./skills/engineering/zoom-out/SKILL.md)：让 agent 从系统层面解释陌生代码
- [`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md)：找出能让代码库更深、更稳、更清晰的架构改进机会

### 小结

软件工程基本功在 AI 时代更重要，而不是更不重要。这个仓库把一组可重复执行的工程实践压缩成 skills，目标是让 agent 更好地参与真实项目，而不是只生成看起来能工作的代码。

## Skills 参考

### Engineering

日常代码工作中使用的 skills。

- **[diagnose](./skills/engineering/diagnose/SKILL.md)**：用于复杂 bug 和性能退化的诊断循环：复现、最小化、假设、插桩、修复、回归测试。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)**：围绕现有领域模型挑战计划，打磨术语，并内联更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)**：用一组 triage 角色状态机处理 issues。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)**：结合 `CONTEXT.md` 里的领域语言和 `docs/adr/` 里的决策，寻找代码库架构加深机会。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)**：为仓库生成 issue tracker、triage 标签词表、领域文档布局等配置。使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 前先运行一次。
- **[tdd](./skills/engineering/tdd/SKILL.md)**：用 red-green-refactor 流程进行测试驱动开发，按垂直切片构建功能或修复 bug。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)**：把计划、规格或 PRD 拆成可以独立领取的 GitHub issues。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)**：把当前对话上下文整理成 PRD，并作为 GitHub issue 提交。不再重新访谈，只综合已有讨论。
- **[zoom-out](./skills/engineering/zoom-out/SKILL.md)**：让 agent 退一步，从系统整体角度解释陌生代码区域。
- **[prototype](./skills/engineering/prototype/SKILL.md)**：构建一次性原型，用于澄清设计。可以是可运行的终端应用，也可以是在同一路由下切换的多种 UI 方案。

### Productivity

通用工作流工具，不局限于代码。

- **[caveman](./skills/productivity/caveman/SKILL.md)**：极简压缩沟通模式，在保留技术准确性的前提下减少 token 消耗。
- **[grill-me](./skills/productivity/grill-me/SKILL.md)**：围绕计划或设计进行高强度追问，直到关键分支都被澄清。
- **[handoff](./skills/productivity/handoff/SKILL.md)**：把当前对话压缩成交接文档，方便另一个 agent 接手。
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.md)**：按正确结构、渐进披露和资源打包方式创建新 skill。

### Misc

不常用但保留的辅助工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)**：为 Claude Code 设置 hooks，在危险 git 命令执行前拦截。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)**：把测试文件里的 `as` 类型断言迁移到 `@total-typescript/shoehorn`。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)**：创建包含章节、题目、答案和讲解的练习目录结构。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)**：配置 Husky pre-commit hooks，集成 lint-staged、Prettier、类型检查和测试。

## 上游项目

本 fork 基于 [mattpocock/skills](https://github.com/mattpocock/skills)。英文原版首页保存在 [README.en.md](./README.en.md)。
