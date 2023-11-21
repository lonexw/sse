# 基础：计算机科学


软件虽“软”，背后却是有严密的逻辑体系作支撑，人类发出的指令，经过编程语言的抽象，到计算机的执行，离不开数学、算法、计算机体系结构等理论知识的发展。

用**可计算性理论**或**图灵生平**作为关键线索，结合了解一下计算机发展史，可以对计算机科学的历史有一个全局的认知，了解计算机是从哪里来的， 又如何发展到今天。你会发现计算机与数学逻辑的渊源，图灵机、信息论与冯诺依曼架构等大批关键词会进入你的脑海。

说不定你会对某块内容产生极大的兴趣和热情。

> 深刻理解事物，要追本溯源（第一性原理），以发展的眼光把握事物的发展史，站在巨人的肩膀，站得高，望得远。

### 基础不稳的缘由

很多计算机专业的学生在本科教育阶段遇到是上课只会读 PPT 的老师，加上同学之间又没有几个对计算机本身都感兴趣的伙伴合作学习或者做项目，就会**因此丧失了对计算机的兴趣**，最后变成突击复习应付考试，并没有把**知识转化为能力**。

还有很一大部分工程师并没有接受过完整的计算机学科训练，对基础知识的学习只是来自于短期培训、自学和工作中的碎片化积累，无法在职业生涯中走的更远。

不过好在软件行业注重实践，网络上亦有优质的学习资源和社区，你完全可以自学成才，基础不扎实并不是无法挽回的问题，亦不依赖什么天赋。

**许多表面上的“天赋”，本质上是思维模式的不同，以及在此基础上随着时间，和滚雪球一样衍生出的“阅历”差异，而不是什么真正的天赋凛异。**（知乎用户）

你可以伴随着好奇心，一点点去深入一件事情背后的原理与逻辑，在实践与应用中逐步搭起的知识框架，这比在课堂上被动接收的一个个孤立的知识片段会坚实且具有生命力许多，从而在知识的实际应用与进一步拓展中显得更加游刃有余，并且在面对未知的事物时有更大的力量与精准度去直击事物的本质。


### 初学入门，兴趣和热爱最重要

如果你对计算机知之甚少，也没有接触过编程，简单、轻松而全面的入门课程是最好的选择，一上来让你去学会某种编程语言，可能会让你从此对编程丢掉信心。

**Harvard CS50's Introduction to Computer Science**

计算机科学和编程艺术的一场轻松之旅，我是 CS50 的爱好者，我愿称之为最棒的计算机科学入门课，每年都会刷最新学年的视频，而且你一定会被主讲教授 **David J. Malan** 的个人魅力感染，而且上完这个课程，你会非常自信的去尝试做软件或继续其他领域知识的学习。_感受一下讲师的热情_：

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/125711BA-D177-4402-BF5E-06357B516253/86FF1B08-C315-4B23-B559-2CB78A48B3A9_2/VWW5Jy500xGg6bxUMo8oaO5xV9LBPsuW8ug9apmBljgz/Image.png)

B 站上面有 CS50 年的课程视频，感谢字幕组做了双语言翻译，少了语言的门槛：
- 2019 年的学期视频，翻译质量不错（疫情前版本）：[CS50 哈佛大学 计算机科学导论 名校公开课【合集·完结】](https://www.bilibili.com/video/av329162758)

- 2021 年版本，机器翻译（疫情版本 😷）：[【2021年哈佛大学】CS50 计算机科学通识 ](https://www.bilibili.com/video/BV1B3411q7oc?p=1)

CS50 每年都会开新的网络课程，如果你英文不错，可以考虑直接参加 2022 年秋季学期，跟哈佛的小伙伴成为同学，一起做项目更有意义：

- 课程注册地址：[https://cs50.harvard.edu/college/2022/fall/](https://cs50.harvard.edu/college/2022/fall/)
- edx.org 有同步的在线课程：[CS50's Introduction to Computer Science](https://www.edx.org/course/introduction-computer-science-harvardx-cs50x)

不用担心自己不懂编程，**跟着做项目是最关键的**。

还有一些非常不错的经典入门课程推荐：

- Udacity CS101：计算机科学导论
   - 课程资源地址：[https://www.cs.virginia.edu/~evans/courses/cs101/](https://www.cs.virginia.edu/~evans/courses/cs101/)
   - 课程内容相对比较简单，你唯一要跨越的就是语言关；
   - 课程内容：构建一个基本的搜索引擎，在实践中学习，有些同学喜欢这种形式；
- CS 61A: The Structure and Interpretation of Computer Programs
   - Berkeley 的经典 CS 入门课，也非常有深度，课程地址：[https://cs61a.org/](https://cs61a.org/)
   - 教材 SICP（Structure and Interpretation of ComputerPrograms），原版：
      - [https://mitpress.mit.edu/sites/default/files/sicp/index.html](https://mitpress.mit.edu/sites/default/files/sicp/index.html)
   - 课程使用 Python3 作为授课的编程语言，Python3 改写的 SICP 教材：
      - [https://composingprograms.com/](https://composingprograms.com/)
   - 教授 John DeNero：[https://denero.org/](https://denero.org/)

额外的一些资源推荐,如果你还在大学校园，应该会感兴趣：

- **csdiy.wiki** 一个北京大学信科专业同学在大三（2021 年）整理的一份**计算机自学指南**，如果你还是学生，想寻找同龄人一起组团学习，这里是不错的选择：[CS自学指南](https://csdiy.wiki/)
- **CrashCouse** 计算机速成课系列视频，可以当成科普和学科概览视频：[【计算机科学速成课】[40集全/精校] ](https://www.bilibili.com/video/av21376839)
- [0xFFFF](https://0xffff.one/p/2-0xffff-intro) 这是一个面向计算机爱好者的学习交流社区，发源于华南师大计算机学院，目前汇聚了一些来自各地高校、企业乃至于中小学的计算机爱好者们。

[Linux C编程一站式学习](https://akaedu.github.io/book/index.html)

[Stanford CS110L: Safety in Systems Programming - CS自学指南](https://csdiy.wiki/%E7%BC%96%E7%A8%8B%E5%85%A5%E9%97%A8/CS110L/)

[Teach Yourself Programming in Ten Years](http://norvig.com/21-days.html)

【视频】Hubris：为健壮性开发操作系统

Hubris 是一个用于深度嵌入计算机系统的小型开源操作系统，例如：我们的服务器替代基板管理控制器（BMC，Baseboard Management Controller）。

本次演讲将概述 Hubris 的设计、Hubris 应用程序的结构以及我们在此过程中学到的一些亮点。

视频链接，[https://talks.osfc.io/osfc2021/talk/JTWYEH/](https://talks.osfc.io/osfc2021/talk/JTWYEH/)

[Teach Yourself Programming in Ten Years](http://norvig.com/21-days.html)

## 文章总结

### 几本技术与人文的书籍推荐：

禅与摩托车的维修艺术

技术呆子

https://brilliant.org/