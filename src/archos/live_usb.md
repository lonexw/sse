# 准备好引导安装的U盘

在使用 **Arch Linux** 打造个性化的工作机器前，我们先使用镜像文件制作启动 U 盘，用作**救援系统、linux 安装程序**或其他系统的基础。

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/F20E09BD-0BB0-4773-9EE1-52948A0E5B4C/B8D9F236-1DA3-4559-AD45-8C0B4B9472BC_2/Hcy45yaaAy7CQOvXPR6POkoEwLoUIX2wlVyBXiaBqP4z/Image.png)

### 1）下载镜像文件

简单的第一步，把镜像文件下载到本地:

- Arch Linux 官方镜像：[https://archlinux.org/download/](https://archlinux.org/download/)
- 阿里云开源镜像站：[https://mirrors.aliyun.com/archlinux/iso/latest/](https://mirrors.aliyun.com/archlinux/iso/latest/)
- 清华大学镜像站：[https://mirrors.ustc.edu.cn/archlinux/iso/latest/](https://mirrors.ustc.edu.cn/archlinux/iso/latest/)

如果担心下载的文件有安全隐患(比如从某网盘或 http 链接上下载)， 可以验证签名：

```other
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
```

**对应 .sig 签名文件可以在官方镜像网站下载，**这一步需要使用到 [GnuPG 命令行工具软件](https://wiki.archlinuxcn.org/wiki/GnuPG)。

```other
pacman-key -v archlinux-version-x86_64.iso.sig
```

图形化的签名验证软件: [GPG Suite](https://gpgtools.org/) on MacOS

[](https://gpgtools.org/)

也可以之后在 Arch Linux 中使用 [**Archiso**](https://wiki.archlinuxcn.org/wiki/Archiso) 构建定制化的 **ISO** 映像，看自己需求是否有必要。

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/F20E09BD-0BB0-4773-9EE1-52948A0E5B4C/14D58BFD-8FF4-4EA4-9901-80C0E831AD87_2/rQZYI7Lb2bGnlDMO7a73ODz7n5MtYGbQ4iMgiNU2GSIz/Image.png)

## 2）制作 USB 系统启动盘

无论是 Window 还是 MacOS 都有不少图形化程序来把镜像写入到 U 盘：

- Rufus： [https://rufus.ie/zh/](https://rufus.ie/zh/)
- USBWriter： [https://sourceforge.net/p/usbwriter/wiki/Documentation/](https://sourceforge.net/p/usbwriter/wiki/Documentation/)
- win32diskimager： [https://sourceforge.net/projects/win32diskimager/](https://sourceforge.net/projects/win32diskimager/)

如果可以使用命令行工具，亦非常简单：

```other
# List block，展示所有块设备：硬盘，闪存盘，CD-ROM等等，不会列出 RAM 盘的信息；
lsblk 
# 写入镜像文件到指定 u 盘
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress && sync

# 在 macos 也有类似命令
diskutil list
sudo dd if=path/to/arch.iso of=/dev/rdiskX bs=1m
```

![Image.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/F20E09BD-0BB0-4773-9EE1-52948A0E5B4C/1E384349-5953-40A2-8886-AA55EB201100_2/1ZaHFktOqAN444L79WyWB8d1V6SZg7bMfCCj0UybErAz/Image.png)

## 3）多系统启动 &  救援系统

### 可以多镜像系统引导的 U 盘

当你有多个操作系统、不同版本的镜像文件， 也不想一遍又一遍的格式化 u 盘，写入新镜像。

开源工具 **Ventoy** 是一个不错的选择： [https://www.ventoy.net/cn/index.html](https://www.ventoy.net/cn/index.html)， 只需要像正常的文件操作，把镜像文件扔到 u 盘就可以，同时 u 盘也可以正常来使用。

![Image.tiff](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/F20E09BD-0BB0-4773-9EE1-52948A0E5B4C/85C63B3F-D71B-40DE-812C-B8BEE4D61A1A_2/qH6OiSLUkizzf36uUZ3IJ6dBlxyYk8vkQRuXowygOt4z/Image.tiff)

使用文档： [https://www.ventoy.net/cn/doc_start.html](https://www.ventoy.net/cn/doc_start.html) 。

### 救援系统

重装系统是很经常的用途， 当系统出现不可预料的异常无法进入时，这个 u 盘也可以作为救援系统来使用，亦可以应付紧急状况的使用。[https://wiki.archlinuxcn.org/wiki/Chroot](https://wiki.archlinuxcn.org/wiki/Chroot)

使用 chroot 是是常见的用法， 改变根目录通常是为了在无法启动或登录的系统上进行系统维护：

- 重新安装 [bootloader](https://wiki.archlinuxcn.org/wiki/Bootloader)。
- 重建 [initramfs 镜像](https://wiki.archlinuxcn.org/wiki/Mkinitcpio)。
- 升级或[降级软件包](https://wiki.archlinuxcn.org/wiki/Downgrading_packages)。
- 重置[忘记的密码](https://wiki.archlinuxcn.org/wiki/Password_recovery)。
- 在[干净的 chroot](https://wiki.archlinuxcn.org/wzh/index.php?title=%E5%9C%A8%E5%B9%B2%E5%87%80%E7%9A%84_chroot_%E7%8E%AF%E5%A2%83%E4%B8%AD%E6%9E%84%E5%BB%BA&action=edit&redlink=1) 中构建软件包。

chroot： [https://wiki.archlinuxcn.org/wiki/Chroot](https://wiki.archlinuxcn.org/wiki/Chroot)

[技术|如何通过 chroot 恢复 Arch Linux 系统](https://linux.cn/article-14708-1.html)

