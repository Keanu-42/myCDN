## Describe the bug

- 我在使用pamac更新软件时遇到报错，报错内容如下：
- Pamac reported errors when I used it to upgrate the software, the error content is as follows: 

![pamac](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/pamac_error.png)

```
正在准备...
正在同步软件包数据库...
正在更新 core.db...
正在更新 extra.db...
正在更新 community.db...
正在更新 multilib.db...
错误: multilib.db: GPGME 错误：无数据
错误: multilib.db: GPGME 错误：无数据
错误: multilib.db: GPGME 错误：无数据
错误: multilib.db: GPGME 错误：无数据
无效或已损坏的数据库 (PGP 签名)
同步数据库失败
无事可做.
事务成功完成.
```

----
> error: multilib.db: GPGME error: no data
> invalid or corrupted database (PGP signature)
----

- 使用pacman更新系统是也同样报错：
- Same problem with pacman when i upgrate my system : 

![pacman_error](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/pacman_error.png)

```
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
:: 正在同步软件包数据库...
 core                  169.6 KiB   508 KiB/s 00:00 [######################] 100%
 extra                1892.8 KiB  7.39 MiB/s 00:00 [######################] 100%
 community               6.6 MiB  9.90 MiB/s 00:01 [######################] 100%
 multilib              174.6 KiB  2.30 MiB/s 00:00 [######################] 100%
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：failed to synchronize all databases (无效或已损坏的数据库 (PGP 签名))
```

----
> error: GPGME error: No data
> error: failed to synchronize all databases (invalid or corrupted database (PGP signature))
----

- 然后我在论坛里看到这篇帖子「[Error: failed to synchronize all databases ...](https://forum.manjaro.org/t/error-failed-to-synchronize-all-databases-invalid-or-corrupted-database-pgp-signature/76320)」，我的情况和帖子里面的一模一样，我以为我找到了解决方案。
- Then I saw this post 「[Error: failed to synchronize all databases ...](https://forum.manjaro.org/t/error-failed-to-synchronize-all-databases-invalid-or-corrupted-database-pgp-signature/76320)」 in the forum, my problem is exactly the same as pjbrunet's and I thought I found the solution .

## To Reproduce

- 根据pjbrunet的描述，我在`var/lib/pacman/sync`该路径下看到了sig文件并把它们删掉，pacman可以正常使用了，但pamac并没有。我再次使用pamac更新软件时，该路径下依然会产生‘.sig’文件，同样的操作在手机热点下结果一样，所以我并不确定是我的网络问题还是pamac的问题。

![d_sigs](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/d_sigs.png)

----

![pacman_work](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/pacman_work.png)

- According to pjbrunet, I saw the '.sig' files in `var/lib/pacman/sync` and deleted them , pacman works fine but pamac  does not. When I used pamac to upgrating the software again, the '.sig'files was still generated in this path. The same operation had the same result with my  mobile's wifi , so I wasn't sure if it was my network or pamac .

## System information

 - OS: Manjaro Gnome 40
 - pamac version: 10.1.3-3
 - Pacman: v6.0.0 - libalpm v13.0.0

## Additional context

- network: campus network
- mirrorlist: global (refresh frequently)

## Solutions

- 这篇帖子「[Error: failed to update multilib ...](https://forum.manjaro.org/t/error-failed-to-update-multilib-invalid-or-corrupted-database-pgp-signature/18008)」基本概括了我的情况，里面提到的解决方法应该是解决了我的问题，虽然还是有点玄学的成分在里面233
- 关于PGP签名更详细的内容请看Arch Wiki的这篇「[pacman / Package signing](https://wiki.archlinux.org/title/Pacman/Package_signing#Troubleshooting)」

----

 > Hi!
 > Try this
 > ```
 > sudo pacman -Sy archlinux-keyring manjaro-keyring
 > sudo pacman-key --populate archlinux manjaro
 > sudo pacman-key --refresh-keys
 > ```
 > If doesn’t work, try this
 > Delete package cache
 > ```
 > pacman -Sc
 > ```
 > Delete as root
 > `/etc/pacman.d/gnupg`
 > Regenerate it with
 >
 > ```
 > sudo pacman-key --init
 > sudo pacman-key --populate archlinux
 > ```
 > > ———— @visone

----

- PGP签名损坏的原因比较多，其中一个可能容易遇到的是系统时间不同步，所以在这里我们要引入“ntp”，“systemd-timesyncd”这两个东西。

- ntp，即网络时间协议（Network Time Protocol），是GNU/Linux系统通过Internet时间服务器同步系统软件时钟的最常见的方法。如果你已经安装了这个程序，那么就可以用这个命令同步时间。

    ```
    sudo ntpd -u ntp:ntp
    ```

- 实际上在大多数情况下，我们并不需要提供 NTP 服务，所以不需要安装ntp软件包，而是使用下一节的方法实现时间同步。

- systemd-timesyncd，是一个用于跨网络同步系统时钟的守护服务，它实现了一个 SNTP 客户端，与 NTP 的复杂实现相比，这个服务简单的多，它只专注于从远程服务器查询然后同步到本地时钟。

- 启用时间同步命令

    ```
    sudo timedatectl set-ntp true
    ```

- 检查时间同步状态

    ```
    timedatectl status
    ```

    ```
    systemctl status systemd-timesyncd
    ```

----

- 上面我们提到了通过重新安装密钥来解决PGP签名损坏的问题，其实这里建议放到最后一步，也就是在同步好系统时间并刷新镜像源之后。

- 关于镜像的问题，我尝试了几种方法效果都不太好，最后还是在Manjaro的论坛里看到这篇帖子「[Pacman-mirrors-fasttrack ...](https://forum.manjaro.org/t/pacman-mirrors-fasttrack-adding-slow-or-invalid-entries-to-mirrorlist/75348)」，并找到了解决方法，就是下面这个命令。

    ```
    sudo pacman-mirrors --method -rank
    ```

- 这条命令在从arch的镜像源里拉取镜像时，给每一条地址做了测速，并按照高低顺序排好了位置。**注意**，刷新完后一定要强制更新一下系统和软件库。

    ```
    sudo pacman -Syyu
    ```

- 按照顺序做完这三步后，不出意外应该可以让系统重新“活过来了”。

    > 折腾Linux真的是让人又爱又恨 233