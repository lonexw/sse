# 基础的系统配置和应用

重启新安装好 Arch Linux 的电脑，用 root 账号登录进入终端环境：

### 配置有线和无线网络

```bash
# 增加有线网络配置 vim /etc/systemd/network/0-wired.network
# 0-wired 的命名可以自己指定，数字开头方便排序
# 基本配置示例，DHCP=yes 来同时接收 IPv4 和 IPv6 DHCP 请求
  [Match]
  Name=en*
  [Network]
  DHCP=yes
# 启用 systemd-networkd、systemd-resolved 服务
systemctl enable --now systemd-networkd.service
systemctl enable --now systemd-resolved.service

# 如果需要连接无线网络，可以使用 iwd（iNet wireless daemon，iNet 无线守护程序）
pacman -S iwd & systemctl enable --now iwd.service
# 搜索并连接验证无线网络，手册：https://wiki.archlinuxcn.org/wiki/Iwd#iwctl
iwctl

# 强制更新所有软件
pacman -Syyu
```

拓展阅读： [Linux ip 命令详解：网络配置工具](https://wangchujiang.com/linux-command/c/ip.html)

### 创建管理员账号

```bash
# 增加新用户账号 lone，并附加管理组 wheel, -m 选项创建用户主目录
useradd -m -G wheel lone
passwd lone
# 获取 root 执行权限，直接 su 切换到 root， 或者安装 sudo 
pacman -S sudo
export EDITOR=vim && visudo # 取消 %wheel  ALL=(ALL) ALL 行的注释
# 用创建后的用户身份登录之后，sudo 执行命令示例
sudo pacman -Sy

# ⚠️ 危险操作：禁止 root 账户登录
passwd -l root     # 解锁： sudo passwd -u root
```

### 区域和本地化的配置

```bash
# 🕰️ 同步更新系统时间, 选择合适的时区信息 Asia/Shanghai
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
# 本地字符集, 支持英文和简体中文
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "zh_CN.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" >> /etc/locale.conf

# 设置 hostname 和 host 文件，记得更换 $hostname
echo "$hostname" >> /etc/hostname
echo "127.0.0.1 localhost" >> /etc/hosts
echo "::1       localhost" >> /etc/hosts
echo "127.0.1.1 $hostname.localdomain $hostname" >> /etc/hosts
```

基本的【中文】支持配置

```bash
# 设置环境变量 vim /etc/environment
# tty 终端支持中文显示是件复杂、无太大意义的事情，涉及修改内核，不建议折腾
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US

# 安装中文字体：Android 设备的字体显示方案，你可以安装自己喜欢的字体
sudo pacman -S ttf-roboto noto-fonts noto-fonts-cjk adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts ttf-dejavu
# 配置示例 https://wiki.archlinuxcn.org/wiki/字体配置/中文
vim ~/.config/fontconfig/fonts.conf 
fc-cache -fv  # 更新字体缓存

# 中文输入法的安装在后续桌面环境配置中说明；
```

### 科学上网能力配置

我们生活在现实世界中，有很多超越技术本身的缺陷， 但是解决掉网络限制问题确实会让后面的很多技术步骤少很多问题。 在这里只记录**如何用 clash（已于 2023.11 停止更新）** 来处理网络代理问题。

```bash
# 从官方仓库下载 clash 核心软件
pacman -S clash
# 引入配置文件，配置目录不存在可以直接创建
# 配置文件示例： https://github.com/MetaCubeX/Clash.Meta/blob/Alpha/docs/config.yaml
# 可用的海外代理服务器需要你自己搞定。
vim ~/.config/clash/config.yaml 
# 让 clash 跟随系统自动启动, lone 代表当前用户，方便 clash 找到对应用户配置文件信息
systemctl enable --now clash@lone

# 设置环境变量，vim /ect/environment
export http_proxy=127.0.0.1:7890
export https_proxy=127.0.0.1:7890
export socks_proxy=127.0.0.1:7891

# 此时就可以 ping google.com 或者 github.com 看是否访问无碍
# 如果访问国外中遇到无法访问的域名或 ip 地址，尝试将其加入配置文件的代理规则，重启 clash 服务
systemctl status/restart clash@lone
```

> 你也可以尝试自己寻找办法解决这个限制，但是请仅在技术范畴，勿跨越雷池。

### 软件依赖管理

Pacman 是 Arch 的软件包管理器， 在安装过程中已经多次使用， [**了解更多使用方式和技巧。**](https://wiki.archlinuxcn.org/wiki/Pacman/%E6%8F%90%E7%A4%BA%E5%92%8C%E6%8A%80%E5%B7%A7)

除了官方维护的软件仓库， 社区维护的用户仓库 **AUR（Arch User Repository）， 甚至可以为自己或团队**[**自建本地仓库**](https://wiki.archlinuxcn.org/wiki/Pacman/%E6%8F%90%E7%A4%BA%E5%92%8C%E6%8A%80%E5%B7%A7#%E8%87%AA%E5%BB%BA%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93)**。**

安装 AUR 辅助工具： [yay](https://github.com/Jguer/yay) or [paru](https://github.com/morganamilo/paru) ：

```bash
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git && cd yay
makepkg -si  # 本地编译构建软件包
```

增加 ArchlinuxCN 社区维护的软件仓库，包含许多中文用户常用软件包：

```bash
# 编辑 /etc/pacman.conf，增加仓库地址配置
[archlinuxcn]
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch

# 刷新密钥
sudo pacman -Sy archlinuxcn-keyring
```

### 系统备份快照(btrfs 文件系统)与挂起到硬盘（即休眠）

使用 LinuxMint 团队开发的 [Timeshift](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Flinuxmint%2Ftimeshift) 来管理快照是个不错的选择， 使用上面装好的 AUR 工具：

```bash
# 安装必要软件包
pacman -S grub-btrfs && yay -S timeshift 

# 手动创建快照
timeshift --create --comments "Fall in archos."
# 重新生成 grub 配置文件时添加快照的入口， 方便故障排查
grub-mkconfig -o /boot/grub/grub.cfg
```

启用休眠到硬盘，首先配置 initramfs，编辑 `/etc/mkinitcpio.conf`, 在 **HOOKS 中加入 resume**

```bash
# resume 要在 udev 之后
HOOKS=(base udev autodetect modconf keyboard keymap consolefont block filesystems resume fsck)
# 重新生成 initramfs
mkinitcpio -P
# 添加内核参数 resume，指定 swap 设备，比如我们分区方案中的是 /dev/sda2
blkid -s UUID -o value /dev/sda2   # 找到设备的 UUID
# 编辑 /etc/default/grub 启动配置
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet resume=UUID={YOUR DEVICE UUID}"
# 顺手也自带的官方主题美化一下 Grub 启动界面
GRUB_THEME="/usr/share/grub/themes/starfield/theme.txt"

# 重新生成 grub 配置文件
grub-mkconfig -o /boot/grub/grub.cfg
# 重启之后，如果需要进入休眠状态
systemctl hibernate
```

#### 日常维护使用需要了解的内容

- Arch Linux 使用 [systemd](https://wiki.archlinuxcn.org/wiki/Systemd) 管理系统服务，简单了解即可掌握；
- 系统维护： Arch 是滚动发行系统，软件包的更新速度很快，用户需要花些时间进行 [系统维护](https://wiki.archlinuxcn.org/wiki/System_maintenance)。
- 如果你对系统安全很关注： [https://wiki.archlinux.org/title/Security](https://wiki.archlinux.org/title/Security)

现在我们拥有了一个开机可用的 Arch Linux，可以手动创建快照，方便出问题随时回滚。

> 跟随阅读后续的内容完成**桌面环境、常见应用和开发环境**的配置工作。
