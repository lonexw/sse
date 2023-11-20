# 构建随处可用的开发环境

保持【开发环境】在各种环境、工作场景中的**一致性**对开发体验是有必要的，就像拥有清爽的工作台，只不过它是虚拟的。

在本方案中，开发环境都放在一个**轻量级、灵活可控**的 Linux 环境：[Arch Linux](https://archlinux.org/)，可以随时快速部署在各种场景。

> Tips：有一个高效、高速、没有限制的互联网连接是前提，事半功倍。

---

# 利用移动设备的算力

有些情况，身边只有移动设备可用，或者想把旧的移动设备利用起来作为开发环境使用，大多数日常使用的移动设备（手机、平板设备）都可以满足基本的内存和算力需求。

#### Android 手机 / 平板 / 鸿蒙 OS

#### 1）无需 ROOT，安装 Termux 应用程序

![Image.tiff](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/955E6802-F3D7-4D17-8F88-9CD51D30A4C6/0844E4AC-F6F5-4021-8D03-0DC297C8189A_2/2KytTUo6fwgcr6sWiTr5gGdPHqTVAFi73UPy490hRg8z/Image.tiff)

![Image.tiff](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/955E6802-F3D7-4D17-8F88-9CD51D30A4C6/828808C7-DD85-4C3B-BF6A-26D7B48CBA81_2/uT87LTTfK9ynGEC8jS1PzQeip4tv64y5eB0O1Cno2GUz/Image.tiff)

![Image.tiff](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/955E6802-F3D7-4D17-8F88-9CD51D30A4C6/EF65435B-EDC4-4B06-A2FC-49C56613041D_2/1o8vGzvvex7iw0oxWBYTV7EALCzkVHhjERVBVUyH8PUz/Image.tiff)

[Termux](https://termux.dev/en/) 是一个终端模拟器，提供基本的 Linux 环境，扩展性也不错。不需要 root 设备，简单下载 APK 文件安装就可以使用。

- 通过 F-Droid 下载安装：[https://f-droid.org/en/packages/com.termux/](https://f-droid.org/en/packages/com.termux/)

#### 2）Termux 基础配置

在 termux 界面中来运行命令来更换软件包的下载源为国内（清华大学）：

```Bash
# 打开软件源配置文件
nano $PREFIX/etc/apt/sources.list 

# The termux repository mirror from TUNA:
deb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main

# 更新系统软件
pkg update
```

移动设备屏幕尺寸受限，可以考虑通过使用其他设备 ssh 的方式远程登录（可选）：

```Bash
# 安装 openssh、nmap（Network Mapper：discover your network）
pkg install nmap openssh

# 启动 ssh 服务端
sshd

# 运行 nmap 检查一下 sshd 的端口 8022 是否状态正常
nmap 

# 获取当前用户：输出类似：u0_a122
whoami

passwd u0_a122 // 设置用户密码
# 获取当前设备在局域网中的内网 IP，建议在路由器管理中将此设备与 IP 进行绑定
ifconfig // 寻找 wlan0 的 inet 字段值


# 在其他设备上进行 ssh 登入
ssh u0_a122@192.168.101.5 -p 8022
```

亦可以配置连接**蓝牙键盘和显示器（成本非常低廉）**，直接转变为桌面 PC：

![IMG_1804.heic](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/7CC633C1-321E-4BD3-9300-954FA8CA5DE1_2/MgmikuGF21UZeyO2h9UxbXluN6lb5HRi6H10VxmS1pEz/IMG_1804.heic)

这里使用的是 [Huawei MatePad Paper（HarmonyOS）](https://www.devicespecifications.com/en/model/054358c7) ，兼容 Android 程序，LoFree 的无线蓝牙键盘，设备与显示器通过无线投屏器镜像，MHL Checker ：你也可以使用 HDMI 转 typec 有线连接，这里如果有连接问题，需自行检索解决。

#### 3）装上 Arch Linux 🚀

```other
# 安装必要的软件工具 🔧
pkg install wget openssl-tool proot tar -y 

hash -r // 清楚命令执行的 hash 缓存表

# 下载安装脚本
wget https://raw.githubusercontent.com/EXALAB/AnLinux-Resources/master/Scripts/Installer/Arch/armhf/arch.sh
# 根据 CPU 架构下载 rootfs 及必要文件
bash arch.sh
```

启动 Arch Linux 环境：

```other
./start-arch.sh

# 第一次进入下面👇命令，fix the pacman-key and network problem
chmod 755 additional.sh && ./additional.sh
```

最终的效果：

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/955E6802-F3D7-4D17-8F88-9CD51D30A4C6/B1FE1472-D467-47C0-9EA8-4A6800F00BCA_2/IsSlUYgb9RJvl5P84vxG3O4s9dEJ72GGA4KwjRypdJUz/Image.png)

#### iOS 与 iPadOS

### SSH Way

iPhone 或 iPad 作为开发设备，**使用 ssh 远程连接到主力设备或者云端服务器**是一个不错的选择，可以避免性能不足和苹果生态带来的局限。

> 拍一张 iPad Pro 连接 Archlinux 主机和安卓设备的照片

[Installing · tmux/tmux Wiki](https://github.com/tmux/tmux/wiki/Installing)

Blink Shell

[Blink Shell is a professional, desktop grade terminal for iOS. With Mosh & SSH clients for iOS, lightning fast and fully customizable. The best terminal for iOS and iPadOS.](https://blink.sh/)

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/955E6802-F3D7-4D17-8F88-9CD51D30A4C6/B9B1B797-A2B5-4ED2-B7E8-6B74BD8336F6_2/4QHm6l1Ax7JXxIn8qkuIiWLVP8U1xylkdEkEPqOWpYQz/Image.png)

如何在 iPad Pro 上面 Code 有很多人分享工作流，有兴趣可以去搜索了解一下。

### 虚拟机的方式运行 Arch Linux

我们也可以在 iOS 设备上面运行虚拟机来运行 Arch Linux。先安装一下 AltStore Server：

[Welcome to AltStore](https://altstore.io/)

安装 UTM 虚拟机应用程序：

[UTM](https://getutm.app/install/)

在程序里面按照提示，安装 ArchLinux 虚拟机即可。

给 iOS 设备增加外设或者连接显示器来增加工作的舒适度和提升工作效率。

[Coding a whole web app ONLY using my Phone.](https://www.youtube.com/watch?v=0KmUoTfGa34)

iPad Pro 的尺寸可能更合适

[My Favourite iPad Pro Accessory: The Raspberry Pi 4](https://www.youtube.com/watch?v=IR6sDcKo3V8&list=WL&index=9)

> #### 不太建议在移动设备上使用桌面环境，以命令后环境为主的工作效率是可以保障的。

# 🏗 主力开发机器

可以用在裸机安装上安装 Arch Linux（或双系统）, 并将此设备作为主力开发机器或者工作站来用，有些非常消耗CPU 性能或图形能力的工作可以交给它来处理。

[【金士顿HX426S15IB2K2/32】金士顿 (Kingston) FURY 32GB(16G×2)套装 DDR4 2666 笔记本内存条 Impact风暴系列 骇客神条【行情 报价 价格 评测】-京东](https://item.jd.com/100008405373.html)

镜像下载（阿里云盘，作为网络不畅时的备选方式）：

- 官方地址：
- 国内镜像：
- 阿里云盘：

使用系统镜像制作 Linux 启动 U 盘是一件非常简单，准备空的 U 盘，选一个来软件来操作：

[Ventoy](https://www.ventoy.net/en/index.html)

[Rufus - Create bootable USB drives the easy way](https://rufus.ie/en/)

[balenaEtcher - Flash OS images to SD cards & USB drives](https://www.balena.io/etcher/)

也可以直接在命令行环境写入：

```Bash
dd
```

---

[WebTorrent - Streaming browser torrent client](https://webtorrent.io/)

如果你的主要工作环境在 Window 或者 MacOS 上面，在虚拟机程序（VMWare or PD）上面运行 Linux 作为体验学习或者处理简单任务可以，但作为工作的开发环境并不完美，推荐下面的方式：

#### MacOS: Docker is better

从官网下载安装 Docker Desktop：

[Get Docker](https://docs.docker.com/get-docker/)

运行 **archlinux:base-devel 容器**：

```other

```

#### Window Linux Subsystem

[GitHub - yuk7/ArchWSL: ArchLinux based WSL Distribution. Supports multiple install.](https://github.com/yuk7/ArchWSL#archwsl)

# ☁️ Work On Cloud or DIY Server

AWS

如何搞定可定制，低成本的 VPS。

国内云服务厂商。

树莓派 DIY Server

[Khadas VIM4 Amlogic A311D2 Single Board Computer with Active Cooling Kit Supports 4K UI and HDMI Input, 4 Display Interfaces, LAN WiFi 6 & Bluetooth 5.1, 8GB 64bit LPDDR4X 2016MHz](https://www.amazon.com/dp/B09ZTFMXDX?tag=robharrop06-20&th=1&keywords=vim4&geniuslink=true)

# 🦾 配置开发环境

> A typical application development environment requires a number of different programs, libraries and tools to produce working software

> ## Goals

- > Eliminate hidden app dependencies.
- > Increase development agility.
- > Improve deployment predictability.
- > security
- > improve code workflow
- > many useful tools

arch for china

sudo user

git dotfiles

for work style

program language

[Dracula — Dark theme for 275+ apps](https://draculatheme.com/)

# make a rust cli app：archdev

# 如何拥有一台计算机？

无论你现在年龄如何，只要对计算机感兴趣，有热情，不妨跟我一道同行，让自己在未来拥有理解科技和动手创造的能力基础。

工程学和数学

是计算机让创作活动普及了，创作不再只是少数人的特权

如果你洞悉了改善计算机技术的秘诀，那么，下一个计算机先驱很可能就是你。

计算机科学也具备自学成才，无师自通的物质基础。

人类天生便有着认识世界、改造世界的天性，只是大部分人都将自己的这一部分天性遗忘掉了而已。如果你失去了这种[内驱力](https://www.zhihu.com/search?q=%E5%86%85%E9%A9%B1%E5%8A%9B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2019321311%7D)，自然也就难以识别与感知到别人在表面的“天赋”后面的努力，可能也难理解别人掌握知识、创造价值背后的欣喜，尽管这种简单的快乐在本质上与孩子在海边堆起沙堡无异。

**英文能力可以边学边用。**

计算机不仅实用，而且造价低廉，凡是你能想到的任何活动中都能找到它们的身影。

> 所以，不要把自己的失败全部归结于天赋，真正陪伴你一生的是坚持和兴趣

这里并不是教你如何使用计算机，这里主要让你更加了解计算机，包括如何编写程序，设计应用解决现实问题，包括理解现在科技社会更复杂的领域，比如云计算、AI、智能机器人这些领域。

拥有一台可以使用的学习和使用的计算机设备是一个前提，就像你要学习剑术，至少需要先有一把拿来练习的剑，而计算机的复杂度更高，需要你对计算机有一个初步的了解。

处在 2021 年这个时间阶段，计算机其实已经无处不在，基本每个人都有一台智能手机，每天都在使用。

[TouchCal](https://www.behance.net/gallery/70895021/TouchCal)

亲自动手来攒机

移动设备（手机/平板）

树莓派的做法

云服务器

自己有笔记本或者台式机

### 学习计算机的基本组成部分和运行原理

从命令行开始是个不错的选择

无论后续你是拥有其他任何性能更好的设备，这个是个不错的开始，随便折腾吧

# Exploring How Computers Work

技术主义的天才，要多思考为什么

# VIM EDITOR

[Home - Neovim](https://neovim.io/)

[Vim Awesome](https://vimawesome.com/)

[Vim Cheat Sheet](https://vim.rtorr.com/lang/zh_cn)

[GitHub - qvacua/vimr: VimR — Neovim GUI for macOS in Swift](https://github.com/qvacua/vimr)

[Home · qvacua/vimr Wiki](https://github.com/qvacua/vimr/wiki)

[Neovim - The Son Of The Son Of VI](https://www.youtube.com/watch?v=tpujjNcFhjw)

[Awesome Neovim Setup From Scratch - Full Guide](https://www.youtube.com/watch?v=JWReY93Vl6g)

---

#### 参考资料

[Termux 高级终端安装使用配置教程](https://www.sqlsec.com/2018/05/termux.html)

[Software - Termux Wiki](https://wiki.termux.com/wiki/Software)

[GitHub - proot-me/proot: chroot, mount --bind, and binfmt_misc without privilege/setup for Linux](https://github.com/proot-me/PRoot)

[Nmap: the Network Mapper - Free Security Scanner](https://nmap.org/)

[Archuro](https://vhs.codeberg.page/code/archuro/)

[cnblogs.com/challa/p/15291249.html](cnblogs.com/challa/p/15291249.html)

