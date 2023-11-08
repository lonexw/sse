# 自定义安装 Arch Linux

### 先搞定网络问题 🛜，避免不少痛苦

[Reflector](https://xyne.dev/projects/reflector/) 是一个 Python 脚本（已默认安装），获取最新的镜像列表，然后筛选出最新的镜像并按速度排序，最后将结果写入到 `/etc/pacman.d/mirrorlist` 文件。

```other
# 从中国地区的镜像中筛选出 5 个最新的并且支持 HTTPS 的镜像
# 然后将结果覆写到 /etc/pacman.d/mirrorlist 文件内；
reflector --country china --protocol https --latest 5 --save /etc/pacman.d/mirrorlist
```

> 若安装过程无网络连接， 可以使用本地镜像仓库的方式**离线安装**。

从 u 盘引导启动机器后， 在命令行键入 [**archinstall**](https://wiki.archlinuxcn.org/wiki/Archinstall) 即可自动化安装新系统:

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/BB2DE5E3-15B3-4162-868B-01076F020BFE/5D9584EE-9936-424E-A176-CC67A113229B_2/xVWRbyIGsgCQqEeS69t80nj1woqLlsnsejyYxFDaWsMz/Image.png)

简单配置后，等几分钟就可以安装成功，重启进入 Arch Linux。

# 手动安装：满足自定义需求及更多乐趣

      #### ::3 步：磁盘分区» 引导程序 » 系统定制::

### 1）磁盘分区

#### 基础概念学习：

- Linux 中的设备文件系统， 了解一下什么是块设备（Block） [https://zh.wikipedia.org/wiki/%E8%AE%BE%E5%A4%87%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F](https://zh.wikipedia.org/wiki/%E8%AE%BE%E5%A4%87%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)
- 文件格式的大概了解： FAT32、NTFS、ext4、btrfs、APFS
- 了解系统引导， 知道 BIOS 与 UEFI 模式是什么[https://wiki.archlinuxcn.org/wiki/UEFI](https://wiki.archlinuxcn.org/wiki/UEFI)
- 理解 Linux 系统的启动过程 [https://wiki.archlinuxcn.org/wiki/Boot_loaders](https://wiki.archlinuxcn.org/wiki/Boot_loaders)

[UEFI - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/UEFI)

[Swap - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Swap#%E4%BA%A4%E6%8D%A2%E6%96%87%E4%BB%B6)

[分区 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E5%88%86%E5%8C%BA#%E5%B8%83%E5%B1%80%E7%A4%BA%E4%BE%8B)

[微码 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E5%BE%AE%E7%A0%81)

[udev - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Udev)

如同装修新居一样， 我们首先要规划设计一下如何使用系统盘的硬盘空间，无论是将系统安装 SATA 硬盘、 U 盘， 还是高速大容量的 SSD 硬盘。

分区方案有一些通用建议，但没有严格的准则，实际的分区方案完全可以根据你的需求来定制：

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/BB2DE5E3-15B3-4162-868B-01076F020BFE/B658B0C3-3220-4A9D-82D2-03FF8A0B0DA8_2/6yOLQwuvhJ995csRwnGBaSs76ZwGKQwXvHAF4QVk61Ez/Image.png)

以需求：将 Arch Linux 安装在一个 2TB 的固态移动硬盘中， 来规划实际的分区方案：

| Device    | 挂载点       | Size 空间 | 类型 Type          | 文件格式  | 备注                                                                                                                                                                                        |
| --------- | --------- | ------- | ---------------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /dev/sda1 | /mnt/boot | 512M    | EFI System       | FAT32 | 引导分区， 至少 300M；如果打算安装多个内核， 或者双系统，最好预留 1GB 以上空间                                                                                                                                             |
| /dev/sda2 | [SWAP]    | 32G     | Linux swap       | swap  | Linux 交换空间，把硬盘空间作为虚拟内存<br>通常为交换分区分配两倍内存大小的空间<br>考虑是否会使用休眠（挂起到硬盘）的功能                                                                                                                       |
| /dev/sda3 | /mnt/     | 512G    | Linux filesystem | btrfs | 新的文件系统 Btrfs，支持写时复制（CoW）、快照以及压缩等高级特性。子卷划分：<br>-  / 根分区：主文件系统挂载， 必需。<br>- /home 独立分区：用户的配置和应用文件。<br>- /.snapshots: 备份快照文件数据存储分区。<br>- /var/log: 系统日志文件，单独分区存储。<br>- /var/cache:  下载的软件包缓存。 |
| /dev/sda4 | /mnt/data | 剩余空间    | Linux filesystem | ext4  | 存放数据，跨操作系统的文件共享                                                                                                                                                                           |

使用 **fdisk** 工具来执行分区， 也有其他工具可以完成该操作， 比如[parted](https://wiki.archlinuxcn.org/wiki/Parted)、cfdisk、gdisk：

```other
# 对挂载硬盘(/dev/sda)进行分区操作，详细操作（m for help）
fdisk /dev/sdb
# 完成后需要格式化分区
mkfs.fat -F 32 /dev/sda1    # 格式化 boot 分区
mkswap /dev/sda2            # 格式化 swap 分区
mkfs.btrfs /dev/sda3        # 格式化主分区
mkfs.ext4 /dev/sda4         # 格式化数据分区
```

对 btrfs 文件格式的主分区进行子卷划分时略微复杂， 需要先挂载根分区，才能创建子卷：

```other
# 先挂载根分区
mount /dev/sda3 /mnt
# 根据实际需求创建子卷
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@log
btrfs subvolume create /mnt/@cache
btrfs subvolume create /mnt/@snapshots
umount /mnt

# 重新挂载所有分区
# noatime 选项可以降低数据读取和写入的访问时间；
# compress 选项可以在数据写入前进行压缩，减少磁盘的写入量，增加磁盘寿命
# 在某些场景下还能优化一些性能，支持的压缩算法有 zlib、lzo 和 zstd，zstd 算法是最快的。
mount -o compress=zstd:1,noatime,subvol=@ /dev/sda3 /mnt
mkdir -p /mnt/{boot/efi,home,.snapshots,var/{cache,log}}
mount -o compress=zstd:1,noatime,subvol=@cache /dev/sda3 /mnt/var/cache
mount -o compress=zstd:1,noatime,subvol=@home /dev/sda3 /mnt/home
mount -o compress=zstd:1,noatime,subvol=@log /dev/sda3 /mnt/var/log
mount -o compress=zstd:1,noatime,subvol=@snapshots /dev/sda3 /mnt/.snapshots
mount /dev/sda1 /mnt/boot/efi
mount /dev/sda4 /mnt/data/
swapon /dev/sda2

# 这些目录将来并不会被保存快照，禁用写时复制：
chattr +C /mnt/var/log /mnt/var/cache /mnt/.snapshots
```

使用 **lsblk /dev/sda** 命令查看完成后的分区信息：

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/BB2DE5E3-15B3-4162-868B-01076F020BFE/09BAA934-6AF9-4D17-AA8C-1C8CDD4A7F5A_2/kwzpWMGLvWyflQIyANAjaVOKnPMNZT1ZZopKBtj655oz/Image.png)

在分区完成之后，安装一些必要的基础软件包后，再进行后续操作：

```other
# 安装基础软件包 base、linux 内核及常规硬件的硬件到目标系统
# 选择计算机的微码包, 提升 CPU 稳定性：intel-ucode or amd-ucode
# 两个常用软件，方便后续操作：git、vim（文本编辑器，看个人喜好）
# (可选) btrfs-progs 包含了很多用于管理 btrfs 文件系统的命令
# 如果启动后只有无线网络可以使用，需要安装 iwctl 来验证登录 
# pacstrap 会拷贝你的镜像站配置文件(/etc/pacman.d/mirrorlist)到新系统
pacstrap -K /mnt base linux linux-firmware intel-ucode git vim btrfs-progs

# 将上一步文件目录挂载信息写入目标系统，未来也可以在此编辑挂载选项
# 用 -U 或 -L 选项设置 UUID 或卷标
genfstab -U /mnt >> /mnt/etc/fstab

# chroot 到新操作系统，
arch-chroot /mnt
# 如果使用 btrfs 文件系统，需要修改 /etc/mkinitcpio.conf, 重新创建一个 Initramfs
  MODULES=(btrfs)
  BINARIES=(/usr/bin/btrfs)
# 根据所有现存的预设文件生成镜像，无修改无需重复此步骤
mkinitcpio -P

# 设置 root 账户密码
passwd
```

### 2）构建系统引导程序

引导加载程序（Boot Loader，又称引导加载器、启动加载器或启动引导器）是由计算机固件（[BIOS](https://zh.wikipedia.org/wiki/BIOS) 或 [UEFI](https://wiki.archlinuxcn.org/wiki/Unified_Extensible_Firmware_Interface)）启动的软件。

引导加载程序必须能够访问内核和 initramfs 映像，否则系统将无法引导。因此，在典型配置中，它必须支持访问 `/boot` 路径。

> 这是至关重要的一步，决定重启后是否可以正常进入系统；

> ::小提示：这部分的操作都需要 chroot 到新系统内操作；::

#### 方案一：systemd-boot

简单 UEFI 引导管理器，包含在 Arch 的 systemd 包， 无需安装就可以使用：

```other
# /boot/efi 为 EFI 系统分区实际挂载点，根据实际情况修改
bootctl install --path=/boot/efi

# 配置启动菜单，详细：https://wiki.archlinuxcn.org/wiki/Systemd-boot
vim /boot/loader/loader.conf 
  # 示例配置内容
  default  arch.conf
  timeout  4
  console-mode max
  editor   no
# 增加启动项, 熟悉之后可以根据实际需求定制
# 这里根分区的文件系统是 btrfs，所以增加了对应的参数
vim /boot/loader/entries/arch.conf
  # 启动项配置内容配置
  title   Arch Linux
  linux   /vmlinuz-linux
  initrd  /initramfs-linux.img
  options root=PARTUUID={YOUR ROOT PARTATION PARTUUID} zswap.enabled=0 rootflags=subvol=@ rw rootfstype=btrfs
# 如何查找某个分区的 PARTUUID, 比如根分区 /dev/sda3
blkid -s PARTUUID -o value /dev/sda3
```

#### 方案二： GRUB 也是一种常见的选择

使用 pacman 安装需要的软件包：

```other
pacman -S grub efibootmgr
```

安装 Grub 引导程序：

```other
# EFI 系统分区目录：/boot/efi, 根据实际需求修改
# 如果是在 Mac 设备上安装，务必加上 --removable 选项
# --recheck 再次检查 device map，即使 /boot/grub/device.map 已经存在
#    每当你添加/删除计算机中的磁盘时都应使用这一选项。
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=ArchOS

# 生成 Grub 配置文件
grub-mkconfig -o /boot/grub/grub.cfg
```

🎉 恭喜，到这里，我们就已经拥有了一个全新、最基础的操作系统。

```other
exit & umount -R /mnt 
reboot # 重启进入新系统，用 root 账号登录 ⚡️
```

### 3）重启之后的软件预安装和配置工作

- 配置有线和无线网络 🛜

```other
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

- 创建管理员账号

```other
# 增加新用户 lone，并附加管理组 wheel, -m 选项创建用户主目录
useradd -m -G wheel lone
passwd lone
# 获取 root 执行权限，直接 su 切换到 root， 或者安装 sudo 
pacman -S sudo
export EDITOR=vim && visudo # 取消 %wheel  ALL=(ALL) ALL 行的注释
# 用户身份登录之后，sudo 执行命令示例
sudo pacman -Sy

# ⚠️ 危险操作：禁止 root 账户登录
passwd -l root     # 解锁： sudo passwd -u root
```

- 区域和本地化的配置

```other
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

- 基本的【中文】支持配置

```other
# 设置环境变量 vim /etc/environment
# tty 终端支持中文显示是件复杂、无太大意义的事情，涉及修改内核，不建议折腾
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US

# 安装中文字体：Android 设备的字体显示方案，你可以安装自己喜欢的字体
sudo pacman -S ttf-roboto noto-fonts noto-fonts-cjk adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts ttf-dejavu
vim ~/.config/fontconfig/fonts.conf # 配置示例 https://wiki.archlinuxcn.org/wiki/字体配置/中文
fc-cache -fv  # 更新字体缓存

# 中文输入法的安装在后续桌面环境配置中说明；
```

- 科学上网能力配置

我们生活在现实世界中，有很多超越技术本身的缺陷， 但是解决掉网络限制问题确实会让后面的很多技术步骤少很多问题。 在这里只记录**如何用 clash（已于 2023.11 停止更新）** 来处理网络代理问题。

```other
# 从官方仓库下载 clash 核心软件
pacman -S clash
# 引入配置文件，配置目录不存在可以直接创建
# 配置文件示例： https://github.com/MetaCubeX/Clash.Meta/blob/Alpha/docs/config.yaml
# 可用的海外代理服务器需要你自己搞定。
vim ~/.config/clash/config.yaml 
# 让 clash 跟随系统自动启动, lone 代表当前用户，方便 clash 找到对应用户配置文件信息
systemctl enable --now clash@lone

# 在终端环节中设置环节变量，让命令执行的网络请求走代理, 最好重新进入一下 shell
export http_proxy=127.0.0.1:7890
export https_proxy=127.0.0.1:7890
export socks_proxy=127.0.0.1:7891

# 此时就可以 ping google.com 或者 github.com 看是否访问无碍
# 如果访问国外中遇到无法访问的域名或 ip 地址，尝试将其加入配置文件的代理规则，重启 clash 服务
systemctl restart clash@lone
```

> 你也可以尝试自己寻找办法解决这个限制，但是请仅在技术范畴，勿跨越雷池。

- 软件依赖管理

Pacman 是 Arch 的软件包管理器， 在安装过程中已经多次使用， [**了解更多使用方式和技巧。**](https://wiki.archlinuxcn.org/wiki/Pacman/%E6%8F%90%E7%A4%BA%E5%92%8C%E6%8A%80%E5%B7%A7)

除了官方维护的软件仓库， 社区维护的用户仓库 **AUR（Arch User Repository）， 甚至可以为自己或团队**[**自建本地仓库**](https://wiki.archlinuxcn.org/wiki/Pacman/%E6%8F%90%E7%A4%BA%E5%92%8C%E6%8A%80%E5%B7%A7#%E8%87%AA%E5%BB%BA%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93)**。**

安装 AUR 辅助工具： [yay](https://github.com/Jguer/yay) or [paru](https://github.com/morganamilo/paru) ：

```other
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git && cd yay
makepkg -si  # 本地编译构建软件包
```

增加 ArchlinuxCN 社区维护的软件仓库，包含许多中文用户常用软件包：

```other
# 编辑 /etc/pacman.conf，增加仓库地址配置
[archlinuxcn]
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch

# 刷新密钥
sudo pacman -Sy archlinuxcn-keyring
```

- 系统备份快照(btrfs文件系统)与挂起到硬盘（休眠）

使用 LinuxMint 团队开发的 [Timeshift](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Flinuxmint%2Ftimeshift) 来管理快照是个不错的选择， 使用上面装好的 AUR 工具：

```other
# 安装必要软件包
yay -S timeshift 
pacman -S grub-btrfs
# 手动创建快照
timeshift --create --comments "Fall in archos."
# 重新生成 grub 配置文件时添加快照的入口， 方便故障排查
grub-mkconfig -o /boot/grub/grub.cfg
```

启用休眠到硬盘，首先配置 initramfs，编辑 `/etc/mkinitcpio.conf`, 在 **HOOKS 中加入 resume**

```other
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

- Arch Linux 使用 [systemd](https://wiki.archlinuxcn.org/wiki/Systemd) 管理系统服务；
- 系统维护： Arch 是滚动发行系统，软件包的更新速度很快，用户需要花些时间进行 [系统维护](https://wiki.archlinuxcn.org/wiki/System_maintenance)。
- 如果你对系统安全很关注： [https://wiki.archlinux.org/title/Security](https://wiki.archlinux.org/title/Security)

现在拥有了一个开机可用的 Arch Linux，可以手动创建快照，方便出问题随时回滚。

跟随阅读后续的内容完成**桌面环境、常见应用和开发环境**的配置工作。

## 参考资料

- [https://github.com/Lichtso/LinuxSetup/blob/master/setup.sh](https://github.com/Lichtso/LinuxSetup/blob/master/setup.sh)

[现代化的 Archlinux 安装，Btrfs、快照、休眠以及更多。 - 少数派](https://sspai.com/post/78916)

在虚拟机上面快速尝试熟悉一下：

[UTM：开源的多面手 macOS 虚拟机（更新到 2023.10.30）](https://zhuanlan.zhihu.com/p/526352487)

额外的情况

在 U 盘上安装： [https://wiki.archlinuxcn.org/wiki/%E5%9C%A8%E5%8F%AF%E7%A7%BB%E5%8A%A8%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%AE%89%E8%A3%85_Arch_Linux](https://wiki.archlinuxcn.org/wiki/%E5%9C%A8%E5%8F%AF%E7%A7%BB%E5%8A%A8%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%AE%89%E8%A3%85_Arch_Linux)

离线安装： [https://wiki.archlinuxcn.org/wiki/%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85](https://wiki.archlinuxcn.org/wiki/%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85)

ssh 远程安装：[https://wiki.archlinuxcn.org/wiki/%E9%80%9A%E8%BF%87_SSH_%E5%AE%89%E8%A3%85_Arch_Linux](https://wiki.archlinuxcn.org/wiki/%E9%80%9A%E8%BF%87_SSH_%E5%AE%89%E8%A3%85_Arch_Linux)

[从现有 Linux 发行版安装 Arch Linux - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E4%BB%8E%E7%8E%B0%E6%9C%89_Linux_%E5%8F%91%E8%A1%8C%E7%89%88%E5%AE%89%E8%A3%85_Arch_Linux)

[性能优化 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96)

[在可移动设备上安装 Arch Linux - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E5%9C%A8%E5%8F%AF%E7%A7%BB%E5%8A%A8%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%AE%89%E8%A3%85_Arch_Linux)

[linux 挂载usb 休眠状态 关闭-掘金](https://juejin.cn/s/linux%20%E6%8C%82%E8%BD%BDusb%20%E4%BC%91%E7%9C%A0%E7%8A%B6%E6%80%81%20%E5%85%B3%E9%97%AD)


