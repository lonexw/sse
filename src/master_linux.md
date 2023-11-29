# 为什么要掌握 Linux

熟悉 Linux 环境有几个显而易见的好处：

1. 加深对操作系统/IO处理/网络编程的理解，对编程和程序架构都有非常大的助力；
2. 让 Linux 成为你强力的工作助手，提升工作效率和解决问题的能力；
3. 熟悉 Linux 也是很多软件岗位的必备技能，更是某些岗位的加分项目；

一旦选择成为软件工程师，Linux 和命令行环境都会伴随着你的职业生涯，甚至比职业生涯还长久，你会喜欢上这些的。

如果你在这个方面走的更深入一些，有良好的系统编程能力，职业选择亦会宽广许多。

### 从什么地方开始

亲自动手装一台 Linux 个人开发机器是最佳的开始，不建议你在个人办公设备或者虚拟机中尝试。这个过程痛苦和乐趣并存，并将学到比你想象中要多的知识，**最关键的是不会再恐惧**。

如果你所在的处境网络状况非常不错，可以考虑从云服务商采购基础的入门云服务器（放在网上的虚拟计算机，通过 ssh 远程连接）来学习，对学生党来说，攒一台树莓派或开发版来折腾更好。

### 安装 ArchLinux 作为个人开发环境

ArchLinux 是一个轻量级、灵活、社区维护的 Linux® 发行版本，它足够简单且与时俱进，商业诉求和历史包袱很少，社区文档非常清晰（中文翻译覆盖率高），是非常好的学习资料。

- [Arch Linux (简体中文) - ArchWiki](https://wiki.archlinux.org/title/Arch_Linux_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

第一步的目标很简单，就是在你的设备安装 ArchLinux 操作系统，这个过程需要搞定的事情：

![Screen Shot 2022-03-12 at 15.16.02.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/125711BA-D177-4402-BF5E-06357B516253/2C3C6E6A-64F5-44A5-803B-7CE7770B5B70_2/gAvv4RfktxEDxRHKUuFZ961wHx6JaALvZaqCK9IiCDkz/Screen%20Shot%202022-03-12%20at%2015.16.02.png)
折腾的目标：
1. 学会使用镜像安装操作系统，理解基本的计算机硬件知识，磁盘格式/分区规划/启动等等；
2. 理解操作系统启动的整个流程：硬件探测、引导加载、内核加载、systemd init 流程等；
3. 学习一些非常基础的终端命令和 Shell 基础，至少达到能在命令行操作的程度；
4. 了解窗口管理器及桌面环境的发展，自己搞定显卡驱动、选择一个桌面环境进行安装、配置；
5. 搞定网络连接，网卡配置、防火墙、DNS，某些情况可以考虑增加网络代理的设置；
6. 学习用户与权限的基础概念，用 sudo 创建一个管理员账户，学会使用 ssh 远程登录计算机；
7. 学会如何在命令行管理文件，就行用鼠标和界面来操作那样；
8. 学会如何进行软件包安装和更新卸载，包括服务的管理；
9. 日常维护的操作：错误定位、漏洞修复、数据备份等等；

在你拥有了一个基础的 Linux 环境之后，就可以根据后续的学习计划，自行配置完整的开发环境。

### 必备的基础知识

📌 **新手提示**

Linux 终端的基本命令（ls、cp、rm 等等）都是 GNU coreutils 工具包提供的，这个网站是对该工具包的详细介绍，逐一分析其中近100个工具的内部实现。

![Image](http://images.phab.xyz/basic_command.png)

[Decoded: GNU coreutils – MaiZure's Projects](http://www.maizure.org/projects/decoded-gnu-coreutils/index.html)

#### 1）命令行文本编辑器 Vim or Emacs

开发者的工作基本都是在文本编辑器下进行的，在命令行下有些编辑器的基础你需要了解一些，并不是说你要掌握所有的编辑器使用，不过我非常建议你在自己的设备上面安装 Vim 和 Emacs，不用看任何教程，花一两个小时熟悉一下这两个编辑器自带的教程是必要的，软件工具层面有非常多的争吵和论战，至于你未来站在那一边，与你自己的喜好和工作环境会有很大关系。

在桌面环境下也有非常不错的免费、强大的编辑器（IDE）：

- Sublime Text
- Visual Studio Code
- Atom
- Notepad++ etc.

#### 2) 网络编程 Network Programming

现在在地球上有无数台个人计算机和移动设备，这个设备如何通过网络连接在一起达到资源共享和通信的目的，程序之间的数据如何传输？即使你未来并不会做相关的工作，理解清楚这些理论知识也是必须的。必须要掌握的知识点：

- Linux 中进程和线程概念，多线程编程实践；

[Life and Death of a Linux Process](https://natanyellin.com/posts/life-and-death-of-a-linux-process/)

- 网络分层模型和 TCP&IP 协议栈，应用层协议；
- 套接字编程实践（高级套接字）；
- IO 模型 (poll(), select(), signal-driven I/O, epoll)；
- 与网络相关的命令和软件工具使用（:netstat and tcpdump）；

如果你对网络层及应用层非常感兴趣，可以拓展了解一下：

- WebSocket：[https://ably.com/blog/introducing-the-websocket-handbook](https://ably.com/blog/introducing-the-websocket-handbook)
- WebRTC：[https://webrtc.org/](https://webrtc.org/)

[19] Linux 命名管道简介: https://goodyduru.github.io/os/2023/09/26/ipc-named-pipes.html
[20] 《套接字》: https://goodyduru.github.io/os/2023/10/03/ipc-unix-domain-sockets.html
[21] 《Unix 信号》: https://goodyduru.github.io/os/2023/10/05/ipc-unix-signals.html

#### 3）Linux Shell 脚本能力

有一定的 **Bash** 及 **Python** 编程能力会让你能解决很多工作中的自动化或者数据处理问题，包括解决问题的方式和途径也会增加不少。

有很多强大的文本处理命令可以在工作过程中遇到了，再逐步扩充学习。

接触到这步，你肯定也会学习不少其他 shell 的使用，比如 zsh，用它来打造称心的终端环境。

### 进阶知识

#### 1）容器、Docker 与 k8s

不必慌张，这些概念并没有听起来那么吓人，你只需要多花点时间了解一些概念，这些技术也更偏运维和部署发布的环境。

极客时间有个付费课程把这块讲的非常清晰，你可以通过这个课程学习（**非广告**）：

[深入剖析Kubernetes_容器_K8s-极客时间](https://time.geekbang.org/column/intro/116)

#### 2）服务器安全和监控

运维方向需要熟悉很多企业级服务的部署和维护（现场环境往往更复杂，集群灾备，混合云等等），以及安全和监控（zibbix、prometheus）领域方向的必备知识。

> 后续补充这块的内容。

#### 3）设备驱动开发、嵌入式开发方向

不太熟悉，感兴趣的同学可以自行搜索。

---

最关键的是：

> **持续的好奇心，废寝忘食的学习、折腾精神必不可少。**

https://learn.microsoft.com/en-us/linux/