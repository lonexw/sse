# 桌面环境及设备驱动

> https://wiki.archlinux.org/title/General_recommendations

首先简单了解下 Linux 中 **图形用户界面（GUI）** 的基本架构：

- 显卡驱动（GPU）与硬件输入
- Display Server 显示服务器（X11、Wayland、Mir）
	- 桌面环境的基础，处理底层的绘图功能，。其他图形程序不直接在屏幕上绘图；相反，它们向显示服务器发送绘图请求，显示服务器为它们在屏幕上绘图
		- 简而言之，为应用程序提供像素访问
		- 需要像素访问的应用程序的另一个示例是屏幕录像机，它们通过显示服务器提供的 API 获取屏幕数据。
	- 负责管理客户端和硬件设备之间的通信
	- 负责绘制鼠标指针并控制其位置
- Window Manager 窗口管理器：管理打开的窗口
	- 控制窗口的大小及其位置（根据您或应用程序的要求）
	- 窗口周围绘制装饰（标题栏和边框称为窗口装饰）
	- 窗口管理器通过与显示服务器通信来完成其工作。
	- 另一个重要功能是 **窗口合成** ：使得应用程序能够执行一些很酷的操作，例如允许透明度、模糊、绘制窗口阴影、在移动/最小化/最大化时为窗口设置动画以及其他视觉效果
- Display Manager 显示管理器：控制用户会话并管理用户认证。
	- gdm / ssdm / lightdm
- Desktop Environment 桌面环境：包括一套集成的应用程序和实用程序。
	- budgie / Cinnamon / Cutefish / Deepin 
	- GNOME / KDE Plasma / LXDE / LXQt / MATE
	- Xfce

桌面环境代表了安装完整图形环境的最简单方法。然而，如果主流桌面环境都不能满足用户的要求，那么用户也可以自由地构建和定制自己的图形环境。一般来说，构建一个自定义环境需要选择一个合适的窗口管理器或混成器，一个任务栏和一些应用程序（一个最小的方案通常包括终端模拟器，文件管理器和文本编辑器）。 

通常由桌面环境提供的其它应用程序有：

- 屏幕锁定器
- Polkit 身份认证组件
- 登出对话框

一些流行的显示管理器有：

GDM （ GNOME 显示管理器(GNOME Display Manager) ）：GNOME 的首选。

SDDM（ 简单桌面显示管理器(Simple Desktop Display Manager) )：KDE 首选。

LightDM：由 Ubuntu 为 Unity 桌面开发。

X11很大程度上充当了客户端和窗口管理器之间的“一个非常糟糕的”通信协议

https://ifmet.cn/posts/cab49185/

The following desktop environments were tested with success

awesome
bspwm
budgie
cinnamon
deepin
dwm
enlightenment
gnome
i3
kde
labwc
lxde
lxqt
mate
maxx
pantheon
qtile
spectrwm
sway
windowmaker
xfce
xmonad
Ly should work with any X desktop environment, and provides basic wayland support (sway works very well, for example).


### 屏幕定制
- 捕获屏幕
- 色温
- 桌面壁纸配置器


### 任务栏
- 通知守护程序
- 音频控制
- 背光控制
- 媒体控制
- 网络管理器
- Power management

### 必要的程序
- 应用程序加载器
- 默认应用程序
- 文件管理器

### 参考资料

- https://linux.cn/article-12068-1.html
- https://linux.cn/article-12773-1.html

复制粘贴的割裂问题必须搞定

