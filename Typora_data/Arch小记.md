## <center>引言</center>

有一段时间没有写东西，最近我在折腾Arch并尝试使用 *[Xmonad](https://wiki.archlinux.org/title/Xmonad)* 这个窗口。

![xmonad_2]()

## <center>正文</center>

### 初识”窗口“

我第一次接触“窗口”其实是在上一次重装系统的时候试了下Manjaro的i3社区版，距今也有好几个月了，当时因为中文字库缺失直接让我劝退，而我也不知道还要单独安装中文字体。现在重新入坑“窗口”有两个原因：一是我挺喜欢“ *[tiling window managers](https://wiki.archlinux.org/title/Window_manager#Tiling_window_managers)* （平铺式窗口管理器）”的这种交互方式；二是“窗口”的美化也很吸引我。”美化“，在英语里有个专门的名词叫” *[ricing](https://thatnixguy.github.io/posts/ricing/)* “，如果不是在TG群里偶然看到了这个科普，不然我是不会把这两个毫不相干的词联想到一起。

----

### 安装字体

说道中文字体，在一个全新的系统中这个的重要程度仅次于更新镜像，我们直接用包管理器安装以下三种即可：

```bash
$ sudo pacman -S 
  adobe-source-han-sans-cn-fonts
  ttf-sarasa-gothic
  ttf-droid
```

> 为什么要急着安装字体？问就是经验之谈（

pacman下载不够快的话可以打开多线程，在`etc/pacman.conf`里把”ParallelDownloads“的注释去掉，字体安装好后要么重启要么手动更新一下：

```bash
# 依次执行以下命令
$ mkfontscale
$ mkfontdir
$ fc-cache
```

查看系统已安装了的中文字体：

```bash
$ fc-list :lang=zh
```

----

### 安装Xmonad

现在Arch不仅自带”archinstall“安装脚本，也有第三方的 *[GUI镜像](https://archlinuxgui.in/)* ，我则是在虚拟机里装了个kde版本的。系统装好后就开始安装”Xmonad“，这里我用的是油管博主 *[DT大佬](https://www.youtube.com/c/DistroTube)* 的安装脚本，使用了他的脚本就可以直接体验到他本人的桌面及配置。而这个 *[脚本](https://gitlab.com/dwt1/dtos)* 是放在gitlab的仓库里，裸连基本没有问题。在开始安装之前先修改一下两个文件：一是把”pkglist“里的”vim“换成”gvim“，这样可以使用剪贴板功能；二是把脚本里的”doom emacs安装“给注释，毕竟没有魔法插件都装不了。在安装过程中如果遇到本地密钥无法被签署的问题请看这个 *[解决方案](https://www.archlinuxcn.org/gnupg-2-1-and-the-pacman-keyring/)* ，安装完毕后重启计算机。


![xmonad_1]()

<center>实际体验还不错～</center>

----

### 禁止mirrorlist更新

在Arch里，需要禁止 *[reflector](https://wiki.archlinux.org/title/Reflector)* ：

```bash
$ sudo systemctl disable reflector.service
```

在Manjaro里，需要禁止pamac的自动更新：

```bash
$ sudo systemctl disable pamac-mirrorlist.timer
# 如果不行，先执行这个
$ sudo systemctl mask pamac-mirrorlist.timer
```

同时，在`etc/pacman-mirrors.conf`里也要看看是否有自动更新的命令，解决了这个问题后，Manjaro这个系统对于我来说基本也就没有什么痛点了。

写得不多，要解决的问题我可能之前就解决了，对于Arch这个系统我目前不太可能作实际使用，但是后面还是会更新一些相关知识的，希望吧。然后提一嘴 *[nixOS](https://nixos.org/)* 这个系统，最近被一位朋友传经布道，不知道会不会成为Linux发行版中的新主流。

<center>未完待续...</center>