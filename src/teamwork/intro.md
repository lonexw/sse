# 协作：代码与项目管理

尝试把目前**代码管理和项目协作**的工具进行使用层面的总结，在这个过程中也有查阅网络上其他人的分享和思考，希望这篇文章的内容能对你的工作有所助力。

首先说明一下自身的情况，之前运营过一家小型的软件外包企业，现在的工作基本以自由职业为主，所以我的需求主要是**对客户项目、团队内部的项目以及个人的兴趣项目**进行管理，主要会涉及以下内容：
- 项目的立项推进
- 需求沟通、开发任务迭代
- 代码管理和发布
- 团队内部知识库、工具流程的建设

这些经验可能对一些小型软件公司亦适用，或许可以作为评估选型的参考。

> 代码托管是件简单而复杂的事情，软件行业的很多问题都来自于规模性和复杂度，这也是现实世界的一种映射。我们需要在工具成本、易用性、灵活性、网络状况（还有一些政治因素考虑，最近几年的世界演化，技术无国界并不是一个物理规律）、是否开源、私有化部署的复杂度等等层面做些考虑。

> 大型技术组织通常有自己代码管理工具体系（以及历史），有非常多的构建发布工具集成、和对某些云服务的集成度，不过我们更关注的是一个工程师个人工作或者小型团队的选择，暂时不考虑这些因素。

### 避免知识的诅咒

为了避免认知偏差，或信息不对称的影响，快速阐述几个基本的概念：

#### 1）版本控制系统 VCS（Version Control System）

版本控制（抑或是**源码管理 Source Code Management**）是软件团队为了追踪和管理代码变化的软件工程实践，相应的版本控制软件可以帮助我们在出现错误的时候回滚之前版本，减少对其他人的工作干扰，是团队**协作开发**、DevOps 团队**构建部署发布流程**不可或缺的基础设施。

> 没有版本控制绝对是一种噩梦般的体验，想一想电脑上无数人工版本记录嵌套文件夹  📁  

常见的版本管理工具：

- **分布式 *Distributed*  VCS**： [Git](https://git-scm.com/)、[Git-LFS（Git extension for versioning large files）](https://git-lfs.github.com/)、[Mercurial](https://www.mercurial-scm.org/) 以及 [Darcs](http://darcs.net/)，客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来，代码工作可以离线、独立/分布式协作；
- **集中化版本管理 Centrallized VCS**: 诸如 [CVS](https://www.nongnu.org/cvs/)、[Subversion（Apache Project）](https://subversion.apache.org/) 以及 **Perforce（Not Free）**，单一集中管理的服务器，保存所有文件的修订版本，而协同工作的人都通过客户端连到这台服务器，取出最新的文件或者提交更新，超大型的项目或者游戏开发有很多会用 Perforce 来管理；

#### 2）Git 的诞生及相关工具

2005 年，因为和托管 Linux 内核代码的商业版本控制系统软件公司 BitKeeper 在版权上出现纠纷，Linux Torvalds 花了两周时间用 C 语言写了一个全新的版本管理系统，在一个月内完成了 Linux 内核代码的迁移，并向全世界免费开源了这个系统 Git，**这是历史的偶然性**。

![Git](http://images.phab.xyz/git.png)

> 脑图在线文档：[https://www.mubucm.com/doc/oYizknWrD7](https://www.mubucm.com/doc/oYizknWrD7) 

### 以项目为起点，代码为资产

我们不需要用一个工具来满足所有的需求，不同的项目对代码管理的要求不一样，某些客观和非技术原因也会影响决策，所以从**项目类型出发**来思考代码管理的具体实践是可行的。

虽然软件代码不像具体的实业生产一样不可逆，可以经常迭代重构，但是把**代码当作最重要的资产**，这个思维最好从一开始就具备，可以帮助下意识的考虑很多事情：

- 将代码视为自己的资产，好比建立一所长久居住的建筑，你会很在意**质量是否可靠**、长期**是否可维护**，是否在将来的某些情况可以**轻松扩展**；
- 资产的保值也不得不考虑，这些代码是否可以给其他人或者世界带来跟多的价值；
- 代码资产的安全性亦不能被忽略（丢失、泄漏或授权等等问题）；

从这两个因素的考虑，我绘制了一下代码管理的决策过程 👇，后面对重要部分进行解释。

![Code](http://images.phab.xyz/code.png)

### 开源项目的运作

这篇文章并不是来谈论开源项目如何运作的，如果以后经验丰富了再写篇文章来分享。

无论是个人还是商业项目，是否要开源运作，取决于个人对开源事业的一个看法和兴趣，而且开源也不是万能的，**不过有很多伟大的软件确实无法在工作或者商业项目中诞生**。

对于开源项目来说，熟悉 Git / GitHub 的使用，融入社区、从大量优秀的开源项目和工程师分享中学习和成长，到慢慢参与为开源世界最贡献，你会发现真正的热爱，以及开源提供的不可替代的价值。

### 是否私有化部署

对于内部项目来说，出于对**代码安全、访问速度、成本**等因素的考虑，除了选择代码托管平台（一般是根据人数和性能限制来收费），**自建 Git Service** 是一个不错的选择，可以根据实际情况降低成本和灵活集成其他服务、精简功能，对人员和项目代码资产的控制也有全部自主权，不过要付出一些必要的平台维护成本和精力。

> 对于我自己的工作场景来说，还会遇到开发者人数经常跟项目进展剧烈波动的情况，代码托管平台的收费模式会有一定困扰，功能繁杂也会增加培训成本，自建 Git Service 可以减少这些层面的麻烦。

大型的软件企业和机构一般都会选择给自己提供云服务的厂商，在云上或者内网进行私有化部署整套产品研发流程产品体系（包括代码管理这部分），来保障研发效率和代码安全。

所以个人开发者或者小型团队，如果有能力的话，**自建 Git Service 是各方面权衡下来可以做的一件事情**，付出的成本主要是服务器和带宽的费用，而且也有很多开源软件来协助做这件事情。

私有化部署 Git Service 的免费开源软件推荐：
- [GitLab Community Edition (CE)](https://about.gitlab.cn/install/)：产品功能完全，大多数企业都会选择；
- 追求精简： [Gitea](https://gitea.io/en-us/)  or  [Gogs](https://gogs.io/)，开发语言都是 Go 语言；
- 其他一些产品： Gitblit 、[RhodeCode CE (Community Edition)](https://rhodecode.com/open-source)、[Code Berg](https://codeberg.org/);
- 包含代码托管功能的工具产品： [Phabricator](https://secure.phabricator.com/) 和 [Gerrit Code Review](https://www.gerritcodereview.com/)；

### 我的选择：Phabricator + Gerrit Code Review

我们在工作中首先遇到的问题是**多代码仓库源同步**的问题：
- 由于网络联通和拉取速度的问题，需要把 GitHub 上面参与的项目或者关注学习的开源项目进行镜像同步，提升使用体验；
- 团队内部的软件项目，根据不同的项目开发状态，对代码评审的要求不一样，我们**通过两种不同的工具来进行思维和实践的区分**，这里对团队来说会增加不少学习成本，需要注意 ⚠️。
- 外部客户的代码经常存放在不同的代码托管服务商，在协作过程中，我们对于需要交付的代码部分会同步到 Phabricator 的 Git Server 上面，方便我们查阅和管理。

代码提交工作如何和项目进度、问题追踪系统无缝衔接，Code Review 工作如何开展？这些问题也非常重要，所以我们的具体策略是这样：
- 低成本采购 1～2 台云服务器，需要 https + CDN 的支持，做好数据备份和安全维护；
- 搭建两套软件服务：
   - **Gerrit Code View** 提供了一套严格的 Code Review 机制，可以协助团队落地代码审阅，它本身也包含了一套代码仓库管理的机制，亦可以把它当作一个简单的 Git Service 来使用；
   - **Phabricator 其实包含一大堆用来进行软件开发管理的应用，有点像瑞士军刀一样**。虽然该项目在 2021.6 月份停止了维护，但是足够成熟，还可以继续用，如果你对 PHP 很熟悉，还可以 Fork 一份自己来学习维护；我们主要用下面的模块：
      - **Diffusion 代码仓库管理**，代码提交与任务可以自然的关联，提升沟通和开发效率；
      - **项目管理：**项目立项、成员管理、背景资料和知识的留存，任务管理，进度和问题追动，看板系统是开发过程中不可或缺的一部分。
      - 知识分享：也把它当作一个简单的博客/知识库工具来用；
- Phabricator 启动了一个 **OAuth2 Server** 来让 Gerrit 可以打通用户登陆，Gerrit 的 Change Review 进展也可以通过集成和功能扩展 Phabricator 的任务面板来。
- Phabricator 也可以与邮件、即时通信软**件（Slack、飞书）**进行集成；
- 在不超过一定人数规模的情况下，两个软件的使用体验和速度都是绝佳的；

我们选择 Phabricator 来作为代码管理的入口，有一些我们自己的使用经历和主观判断，如果觉得不合适，也可以考虑用自己喜欢的项目管理软件来替代（能私有化部署最好），而且很多功能齐全的代码托管平台本身会把这些功能都整合进去，是否好用，就需要你实际体验一下了。

> 我们团队面板：https://phab.xyz

> 这里还遗漏了两块比较重要的部分：**代码管理策略和 DevOps（CI/CD 等等）**的内容，改日再聊。


### 调试与测试

 远程调试：https://github.com/HuolalaTech/page-spy-web

### 🔐 安全

自动化检查项目中是否有安全密钥泄漏风险：
- https://github.com/sirwart/ripsecrets

#### 参考资料

- [谷歌的代码管理 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/07/google-monolithic-source-repository.html)
- [Why Google Stores Billions of Lines of Code in a Single Repository.pdf](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/254E7392-3F48-4C50-AE59-6AD24E6DBB6F/557FC6B7-5F9D-4E0E-B453-33797BFFE133_2/VngPckMWTPOyzBxLEyiALTo12GZaMtAxN8xrapRp36Ez/Why%20Google%20Stores%20Billions%20of%20Lines%20of%20Code%20in%20a%20Single%20Repository.pdf)
- [GitHub - google/styleguide: Style guides for Google-originated open-source projects](https://github.com/google/styleguide)
- [推荐的代码管理规范 – ifeegoo](https://www.ifeegoo.com/recommended-code-management-specification.html)
- [What is version control | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/what-is-version-control)

- [Mercurial vs Git - Let’s Examine - Incredibuild](https://www.incredibuild.com/blog/mercurial-vs-git-lets-examine)