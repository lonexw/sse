# 工作流程

公司使用 Git 作为源码管理系统，托管在[[ [https://dev.tencent.com/](https://dev.tencent.com/) | 腾讯开发者平台（Coding） ]]，不可避免涉及到多人协作。**协作必须有一个规范的工作流程**，让大家有效地合作，使得项目井井有条地发展下去。"工作流程"在英语里，叫做"workflow"或者"flow"，原意是水流，比喻项目像水流那样，顺畅、自然地向前流动 ，不会发生冲击、对撞、甚至漩涡。

公司建议使用 Sublime Merge 工具来进行 Git 工作流程的管理。如果你喜欢或者使用过 Sublime Text，那肯定也会喜欢 [[ [https://www.sublimemerge.com/](https://www.sublimemerge.com/) | Sublime Merge ]]。

![Image](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/48E934F0-63D6-4198-918E-431B40B43EFC/EEDDB62E-8E67-4767-8998-6F1A3D1759C2_2/85DFyoVQHfYXp8tbFSNbrtLfPgrf4te4qhxVAfVlcP4z/Image)

在了解具体的流程说明之前，需要先了解一下Git Workflow 的**基本原则**：

> "功能驱动式开发"（Feature-driven development，简称FDD）

它指的是，**需求是开发的起点**，先有需求再有功能分支（feature branch）或者补丁分支（hotfix branch）。完成开发后，该分支就合并到主分支，然后被删除。

#### 学习使用 / Learn

虽然说使用 Sublime Merge 工具之后，可以不需要掌握太多 git 命令行，但是基础的概念和命令还是**必须完全掌握**，有助于理解后续的工作流程说明和紧急状况使用。

#### 基础术语

- Workspace : 工作区
- Index / Stage : 暂存区
- Repository ：仓库区
- Remote ：远程仓库
- Branch ：分支

#### 常用命令

![Image](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/48E934F0-63D6-4198-918E-431B40B43EFC/FE07BF7C-AE33-4B2E-B9F5-7F2C0BB8F31D_2/9OPLeazI8L5Yayh3TLXrpphZwCn1vzI5PdkR8krqNTYz/Image)

一般来说，只要掌握以上的基本命令，就可以满足日常使用，其他的命令用的到时候查阅文档就可以，使用多了自然会熟练起来，不用担心记不住命令。

入门选手请查阅：

- git - 简明指南: [http://rogerdudler.github.io/git-guide/index.zh.html](http://rogerdudler.github.io/git-guide/index.zh.html)
- 常用 Git 命令清单: [http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)

#### 分支管理策略

[[ [https://nvie.com/about/](https://nvie.com/about/) | Vincent Driessen ]] 提出了一个[[ [https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/) | 分支管理的策略 ]]，我觉得非常值得借鉴。它可以使得版本库的演进保持简洁，主干清晰，各个分支各司其职、井井有条。

![Image](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/48E934F0-63D6-4198-918E-431B40B43EFC/B705B6B2-4991-4CFE-8E1D-726D31C5D009_2/yDMuskIyNGV7O5fwR8GSk4RSHsyTRB5BKy9dYrxtOmAz/Image)

公司也采用这种分支管理策略。

常设分支：

- **master 主分支**：所有提供给用户使用的正式版本，都在这个主分支上发布。
- **develop 开发分支**：日常开发使用，最新的开发进度分支。

临时性分支：

- Feature 功能分支；
- Release 预发布分支；
- Bugfix 缺陷修复分支；
- hotfix 线上缺陷紧急修复分支。

#### Pro Git

深入了解 Git 原理和相关操作解释。

- Git远程操作详解：[http://www.ruanyifeng.com/blog/2014/06/git_remote.html](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
- ProGit Online Book：[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)

#### 团队协作规范 / Git Workflow

团队开发中，遵循一个合理、清晰的 Git 使用流程，是非常重要的。 这部分的内容是 Yousails Software 团队内部使用的 git 协作流程说明。

#### 仓库维护要则

- 必须避免在代码仓库中带入与环境、开发配置、生产编译结果相关的文件；
- 代码合并后需要及时删除无用的本地和远程分支；
- 非紧急情况下，不要再 develop 或 master 分支下工作，提交代码；
- 定期 rebase，合并上游代码变化；
- 学会使用 PR 来进行 Code Review 和问题讨论、寻求帮助；（在 LKB 系统上进行）

#### 开发阶段（项目未对外发布）

在开发阶段整个开发协作流程采用 GitHub Flow 规范：

![Image](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/48E934F0-63D6-4198-918E-431B40B43EFC/CE5D7FA0-2A27-4A21-9C14-AF71C4232C2D_2/5Rj7kll9rWnwqF8Is9DwgoRcowKNUBMhMyatTWGxmWcz/Image)

Understanding the GitHub flow: [https://guides.github.com/introduction/flow/](https://guides.github.com/introduction/flow/)

#### 流程说明

1 ）只有两个长期分支 master 和 develop，master 分支作为预发布环境（真实用户测试环境 stage 环境），需要和 develop 分支保持一致，develop 分支作为开发环境的代码，永远是可发布状态。

> master 和 develop 分支需要设置分支保护，只有有权限的人才能直接推送代码；
2 ）如果有新功能开发，可以从 develop 分支上检出新分支。
3 ）在本地分支提交代码，并且保证按时向远程仓库推送代码。
4 ）当需要反馈或者帮助，或者需要合并分支时，需要发起 pull request。

> PR（Pull Request）是 Github Workflow 伟大的发明，不仅可以用来合并分支，还可以用来问题讨论、Code Review。
5 ）当 review 或者讨论通过后，代码会合并到目标分支（develop）。
6 ）当完成 Sprint 开发任务或需要向 stage 环境交付代码时，需要合并 develop 分支代码到 master，发布给用户。

#### 开发新功能

基于 develop 分支的最新代码创建功能分支（也可以成为本地分支）：

```other
# Write a feature
git checkout develop
git pull
git checkout -b <branch-name>

# 分支名建议具备一定的描述性，比如
# refactor-authentication、user-content-cache-key、make-retina-avatars
```

经常性 rebase 合并其他人的修改，保证和上游分支一致:

```other
git fetch
git rebase origin/develop
```

当功能开发完毕并且通过了测试，提前之前最好进行一次 rebase，解决潜在的代码冲突，然后提交所有修改记录到本地仓库：

```other
git add --all
git status
git commit --verbose

# Commit 提交信息示例
完成订单自动同步 ERP 功能

- 修改订单数据接口
- 完成 ERP 接口对接和调试
- 完成订单状态的同步

Link: lkb.work/T328
```

功能分支开发完成后，很可能会有一大堆 commit，有些其实并没有参考价值，合并到主分支的时候，往往希望只有一个（或最多两三个）commit，这样不仅清晰，也容易管理。

```other
# 交互式的合并 commit 信息
# https://help.github.com/articles/using-git-rebase-on-the-command-line/
git rebase -i origin/develop
```

推送代码到远程仓库（代码推送之前建议进行一下 rebase 操作，保持与 develop 分支同步）

```other
# 加上 force 参数，因为 rebase 以后，分支历史改变了，与远程分支不一定兼容，可能要强行推送
git push --force origin <branch-name>
```

在代码托管平台中，提交合并请求 ，在项目沟通群众告知相关人员进行代码 Review 和合并部署操作。

> Coding 提供了可以通过命令行创建 Merge Request 的方法：

```other
git push origin <branch-name>:mr/develop/<branch-name>

# 自动创建的使用最后一个『commit message』作为标题和内容
# 可以使用 @Seaony 自动关联相关人员为评审者
# 自动 # 加为关联资源
```

#### 审阅代码 /  Code Review

使用命令行或 Sublime Merge 都可以进行代码修改变动的比较和查看。代码层面的意见反馈和讨论，可以直接在代码托管工具和即时通讯工具上面进行。

![Image](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/48E934F0-63D6-4198-918E-431B40B43EFC/5A51E43D-CFAE-4E5B-8931-E817A08DD071_2/c5291x9ebQ1oREF1OjmKqtmbPl5r93Z6yxJr3MTySZ4z/Image)

#### 编码标准

[https://learnku.com/docs/laravel-specification/5.5](https://learnku.com/docs/laravel-specification/5.5)

[https://learnku.com/docs/psr](https://learnku.com/docs/psr)

[https://learnku.com/articles/25020](https://learnku.com/articles/25020)

#### Code Review 指南

Thoughtbot：[https://github.com/thoughtbot/guides/tree/master/code-review](https://github.com/thoughtbot/guides/tree/master/code-review)

基本原则

- 认同大多数编码决策与观念有关，权衡利弊，快速确定解决方案
- 提好问题，而不是提请求（提问的智慧）
- Ask for clarification. ("I didn't understand. Can you clarify?")
- Avoid selective ownership of code. ("mine", "not mine", "yours")
- Avoid using terms that could be seen as referring to personal traits. ("dumb",
- "stupid"). Assume everyone is intelligent and well-meaning.
- Don't use hyperbole. ("always", "never", "endlessly", "nothing")
- 不要讽刺别人
- 如果有太多其他人不理解的地方，或者讨论篇幅过长，可以进行同步讨论（聊天室、语音会议、屏幕分享等）

自己的代码被审阅

- 友好的回复审阅者的建议（建议不错，我去修改一下）。
- 对事不对人，如果看到咄咄逼人或者带有个人色彩的评论，如果可以的话，及时在线问清楚对方的意图，不要自我揣测。
- Assume the best intention from the reviewer's comments.
- 保持代码的可读性（让代码自己解释为什么存在的原因）
- Push commits based on earlier rounds of feedback as isolated commits to the branch. Do not squash until the branch is ready to merge. Reviewers should be able to read individual updates based on their earlier feedback.
- 及时回复审阅建议
- Wait to merge the branch until continuous integration (TDDium, Travis CI, CircleCI, etc.) tells you the test suite is green in the branch.
- Merge once you feel confident in the code and its impact on the project.

审阅他人的代码

> 审阅者必须先理解 PR 的上下文（修复缺陷、提升用户体验、重构已有代码）；

- Communicate which ideas you feel strongly about and those you don't.
- 找出更简化的代码实现方式，并提供建议。
- 如果需要讨论的问题过于学术化，或者架构的调整，线下的下午茶讨论或许更合适。
- 提供替代的实现方案时，但是假设提交者已经考虑了他们（“你觉得在这里使用自定义验证器怎么样？”）。
- Seek to understand the author's perspective.
- Sign off on the pull request with a 👍🏾 or "Ready to merge" comment.
- **Remember that you are here to provide feedback, not to be a gatekeeper.**

#### 合并分支 / Merge Request

合并分支的时候，避免选择 Fast-Forward 合并方式，而是策略合并，策略合并会让我们多一个合并提交。这样做的好处是保证一个非常清晰的提交历史，可以看到被合并分支的存在。

拥有合并权限的开发同学，也可以通过命令行进行合并：

```other
git log origin/develop..<branch-name>
git diff --stat origin/develop
git checkout develop
git merge <branch-name> --ff-only
git push
```

已经合并的功能分支需要及时进行删除

```other
# delete remote feature branch
git push origin --delete <branch-name>

# delete local feature branch
git branch --delete <branch-name>
```

#### 迭代阶段（项目已上线使用）

此部分内容待更新，该流程主要区别在于命名和分支管理方案上与开发阶段有所差异：

- 功能开发
- 缺陷修复
- hotfix 流程
- 预发布流程
- Issue 追踪管理

团队使用 Phabricator 进行缺陷追踪和管理。

#### 参考资料

- Git Ready [http://gitready.com/](http://gitready.com/)
- 使用原理视角看 Git [https://dev.tencent.com/help/doc/practice/git-principle.html](https://dev.tencent.com/help/doc/practice/git-principle.html)
- 大话 Git 工作流 [https://blog.coding.net/blog/git-workflow](https://blog.coding.net/blog/git-workflow)
- 使用 Feature Branch Workflow [https://dev.tencent.com/help/doc/practice/git-feature-branch.html](https://dev.tencent.com/help/doc/practice/git-feature-branch.html)

