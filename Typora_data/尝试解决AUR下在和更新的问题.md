因为大陆这边对git的连接不是很好，所以我平时都是挂着代理下载和更新AUR，但并不是一直都有用，所以这次尝试从根本解决这个问题 : )

碰巧我在网上看到了这篇[帖子](https://zhuanlan.zhihu.com/p/176987140)，里面提到的方法是修改`/etc/makepkg.conf`使用自定义脚本通过镜像网站和axel多线程下载。

- 修改前

```bash
  12   │ DLAGENTS=('file::/usr/bin/curl -qgC - -o %o %u'
  13   │           'ftp::/usr/bin/curl -qgfC - --ftp-pasv --retry 3 --retry-delay 3 -o %o %u'
  14   │           'http::/usr/bin/curl -qgb "" -fLC - --retry 3 --retry-delay 3 -o %o %u'
  15   │           'https::/usr/bin/curl -qgb "" -fLC - --retry 3 --retry-delay 3 -o %o %u'
  16   │           'rsync::/usr/bin/rsync --no-motd -z %u %o'
  17   │           'scp::/usr/bin/scp -C %u %o')
```

- 修改后

```bash
  12   │ DLAGENTS=('file::/usr/bin/curl -qgC - -o %o %u'
  13   │           'ftp::/usr/bin/axel -n 15 -a -o %o %u'
  14   │           'http::/usr/bin/axel -n 15 -a -o %o %u'
  15   │           'https::/home/sky/Internet/fake_curl_makepkg %o %u'
  16   │           'rsync::/usr/bin/rsync --no-motd -z %u %o'
  17   │           'scp::/usr/bin/scp -C %u %o')
```

- 脚本文件（fake_curl_makepkg.sh）

```bash
   1   │ #! /bin/bash
   2   │ # 该脚本用于处理yay安装软件时，由github下载缓慢甚至无法下载的问题
   3   │ # 检测域名是不是github，如果是，则替换为镜像网站，依旧使用curl下载
   4   │ # 如果不是github则采用axel代替curl进行15线程下载
   5   │ # 实验用链接：
   6   │ # https://download.fastgit.org/beekeeper-studio/beekeeper-studio/releases/download/v1.6.11/beekeeper-studio_1.6.11_amd64.deb
   7   │ # https://github.com/beekeeper-studio/beekeeper-studio/releases/download/v1.6.11/beekeeper-studio_1.6.11_amd64.deb
   8   │ 
   9   │ domin=`echo $2 | cut -f3 -d'/'`;
  10   │ others=`echo $2 | cut -f4- -d'/'`;
  11   │ case "$domin" in 
  12   │     "github.com")
  13   │     url="https://hub.0z.gs/"$others;
  14   │     echo "download from github mirror $url";
  15   │     /usr/bin/curl -gqb "" -fLC - --retry 3 --retry-delay 3 -o $1 $url;
  16   │     ;;
  17   │     *)
  18   │     url=$2;
  19   │     /usr/bin/axel -n 15 -a -o $1 $url;
  20   │     ;;
  21   │ esac
```

修改完并重新登录后，使用`pamac update`进行更新，会提示该脚本未安装：

```bash
正在准备...
正在克隆 electron-netease-cloud-music 构建文件...
生成 electron-netease-cloud-music 信息...
正在检查 electron-netease-cloud-music 依赖关系...
正在克隆 gnome-shell-extension-blur-my-shell-git 构建文件...
生成 gnome-shell-extension-blur-my-shell-git 信息...
正在检查 gnome-shell-extension-blur-my-shell-git 依赖关系...
正在同步软件包数据库...
正在更新 AUR...                                                                                         
正在解决依赖关系...                                                                                     
正在检查内部冲突...

构建 (2):
  electron-netease-cloud-music             0.9.33-1           (0.9.32-1)          AUR
  gnome-shell-extension-blur-my-shell-git  27.r25.gb02e3e0-1  (27.r2.g6d1e992-1)  AUR


编辑构建文件 : [e] 
应用事务 ? [e/y/N] y


正在构建 electron-netease-cloud-music...
==> 正在创建软件包：electron-netease-cloud-music 0.9.33-1 (2022年01月29日 星期六 10时10分11秒)
==> 正在检查运行时依赖关系...
==> 正在检查编译时依赖关系
==> 获取源代码...
==> 错误： 下载程序 fake_curl_makepkg 没有安装。
    正在放弃...
错误: 构建 electron-netease-cloud-music 失败
```

希望各位Linux大佬可以指点一下我这个菜鸟[苦笑]

> 如果这个办法行不通，我只有用第二种办法：
>> 在开始下载更新时，编辑`pkgbuild`文件使用镜像网站

----

<center>回复</center>

----

> @lance6716
> 
> “脚本未安装”可能说的是结果而不是原因？有没有试过手动传参数运行一下
> > 没试过，请问一下大佬我应该怎么做？

> @lance6716
> 
> 看上去你这个脚本的第一个参数是期望保存的文件名，第二个参数是下载地址，就用注释里的例子`https://github.com/beekeeper-studio/beekeeper-studio/releases/download/v1.6.11/beekeeper-studio_1.6.11_amd64.deb`试试呗
>> 脚本是可以用的。
>>> 老哥，我每次更新系统的时候都会有这个提示，“警告： os-prober will be executed to detect other bootable partitions.”，我看网上的资料是在`etc/default/grub`里添加`GRUB_DISABLE_OS_PROBER=false`，问题是我这里已经有了。顺便问一下pamac的缓存位置在哪，有没有清理的必要（

> @douglarek
> 
> 这个警告不用管，没有任何的影响。你添加的那个`GRUB_DISABLE_OS_PROBER=false`恰好就是这个警告的来源，你可以设置为`true`消除这个警告，但是如果你电脑里面同时有windows和linux，那么`grub2`只会引导linux，不再引导windows。pamac缓存你可以使用`pamac clean`清理，有没有必要看你硬盘大小，硬盘大可以不用着急清理，你可以设定保存的版本数。
>> 嗯，了解，我现在确实是Win + Linux，那就忽略这个警告吧。

----