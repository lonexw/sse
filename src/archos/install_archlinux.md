# 个性化安装 Arch Linux

从 u 盘引导启动电脑进入 Live 环境后，**先搞定网络问题，避免不少痛苦：**

[Reflector](https://xyne.dev/projects/reflector/) 是一个 Python 脚本（已默认安装），获取最新的镜像列表，筛选出最新的镜像并按速度排序，最后将结果写入到 `/etc/pacman.d/mirrorlist` 文件。

```other
# 从中国地区的镜像中筛选出 5 个最新且支持 HTTPS 的镜像;
# 然后将结果覆写到 /etc/pacman.d/mirrorlist 文件内；
reflector --country china --protocol https --latest 5 --save /etc/pacman.d/mirrorlist
```

> 若安装过程无可用网络，可以使用本地镜像仓库的方式**离线安装**。

## 安装脚本

在命令行键入 [**archinstall**](https://wiki.archlinuxcn.org/wiki/Archinstall) 即可自动化安装新系统:

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/BB2DE5E3-15B3-4162-868B-01076F020BFE/5D9584EE-9936-424E-A176-CC67A113229B_2/xVWRbyIGsgCQqEeS69t80nj1woqLlsnsejyYxFDaWsMz/Image.png)

简单配置后，确认安装，等几分钟就可以安装成功，重启进入 Arch Linux。

## 手动安装：满足自定义需求及更多乐趣

### 磁盘分区

如同装修新居一样， 我们首先要规划设计一下系统盘的硬盘空间，无论是将系统安装 SATA 硬盘、 U 盘， 还是高速大容量的 SSD 硬盘。

分区方案有一些通用建议，但没有严格准则，实际的分区方案完全可以根据实际需求来定制：

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/BB2DE5E3-15B3-4162-868B-01076F020BFE/B658B0C3-3220-4A9D-82D2-03FF8A0B0DA8_2/6yOLQwuvhJ995csRwnGBaSs76ZwGKQwXvHAF4QVk61Ez/Image.png)

示例需求：将 Arch Linux 安装在一个 2TB 的固态移动硬盘中， 来规划分区方案：

| Device    | 挂载点       | Size 空间 | 类型 Type          | 文件格式  | 备注                                                                                                                                                                                        |
| --------- | --------- | ------- | ---------------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /dev/sda1 | /mnt/boot | 512M    | EFI System       | FAT32 | 引导分区， 至少 300M；如果打算安装多个内核， 或者双系统，最好预留 1GB 以上空间                                                                                                                                             |
| /dev/sda2 | [SWAP]    | 32G     | Linux swap       | swap  | Linux 交换空间，把硬盘空间作为虚拟内存<br>通常为交换分区分配两倍内存大小的空间<br>考虑是否会使用休眠（挂起到硬盘）的功能                                                                                                                       |
| /dev/sda3 | /mnt/     | 512G    | Linux filesystem | btrfs | 新的文件系统 Btrfs，支持写时复制（CoW）、快照以及压缩等高级特性。子卷划分：<br>-  / 根分区：主文件系统挂载， 必需。<br>- /home 独立分区：用户的配置和应用文件。<br>- /.snapshots: 备份快照文件数据存储分区。<br>- /var/log: 系统日志文件，单独分区存储。<br>- /var/cache:  下载的软件包缓存。 |
| /dev/sda4 | /mnt/data | 剩余空间    | Linux filesystem | ext4  | 存放数据，跨操作系统的文件共享                                                                                                                                                                           |

这里使用 **fdisk** 工具来执行分区， 也有其他工具可以完成该操作， 比如 [parted](https://wiki.archlinuxcn.org/wiki/Parted)、cfdisk、gdisk：

```bash
# 对挂载硬盘(/dev/sda)进行分区操作，详细操作（m for help）
fdisk /dev/sdb
# 完成后, 格式化分区
mkfs.fat -F 32 /dev/sda1    # 格式化 boot 分区
mkswap /dev/sda2            # 格式化 swap 分区
mkfs.btrfs /dev/sda3        # 格式化主分区(根分区)
mkfs.ext4 /dev/sda4         # 格式化数据分区
```

对 btrfs 文件格式的主分区进行子卷划分时略微复杂， 需要先挂载根分区，才能创建子卷：

```bash
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

```bash
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

# chroot 到新操作系统
arch-chroot /mnt
# 如果使用 btrfs 文件系统，需要修改 /etc/mkinitcpio.conf, 重新创建 Initramfs
  MODULES=(btrfs)
  BINARIES=(/usr/bin/btrfs)
# 根据预设文件生成镜像，无修改无需重复此步骤
mkinitcpio -P

# 设置 root 账户密码
passwd
```

我这里的系统是安装在可移动硬盘的，多做一步操作，避免 USB 省电模式导致卡死：

```bash
sudo echo -1 > /sys/module/usbcore/parameters/autosuspend
```

### 构建系统引导程序

> 小提示：这部分的操作都需要 chroot 到新系统内操作；

引导加载程序（Boot Loader，又称引导加载器、启动加载器或启动引导器）是由计算机固件（[BIOS](https://zh.wikipedia.org/wiki/BIOS) 或 [UEFI](https://wiki.archlinuxcn.org/wiki/Unified_Extensible_Firmware_Interface)）启动的软件。

引导加载程序必须能够访问内核和 initramfs 映像，否则系统将无法引导。因此，在典型配置中，它必须支持访问 `/boot` 路径。

这是至关重要的一步，决定重启后是否可以正常进入系统；

#### 方案一：systemd-boot

简单 UEFI 引导管理器，已包含在 Arch 的 systemd 软件包， 无需安装就可以使用：

```bash
# /boot/efi 为 EFI 系统分区实际挂载点，可以根据实际情况修改
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

```bash
pacman -S grub efibootmgr
```

安装 Grub 引导程序：

```bash
# EFI 系统分区目录：/boot/efi, 根据实际需求修改
# 如果是在 Mac 设备上安装，务必加上 --removable 选项
# --recheck 再次检查 device map，即使 /boot/grub/device.map 已经存在, 每当你添加/删除计算机中的磁盘时都应使用这一选项
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=ArchOS

# 生成 Grub 配置文件
grub-mkconfig -o /boot/grub/grub.cfg
```

#### 🎉 恭喜，到这里，你拥有了全新、基础的 Linux 操作系统。

```bash
exit & umount -R /mnt 
reboot # 重启进入新系统，用 root 账号登录 ⚡️
```