## <center>引言</center>

最近两天过得很充实，我在两个开源社区收获颇丰。如标题所说，这次我主要解决了三个问题，在这里有必要简单地总结一下 : )

## <center>正文</center>

### git签名

前几天我随手查看了一下过往所有的提交记录发现了一个很奇怪的事，凡是我在本地的提交都没有“Verified”，也就是签名验证，仔细了解了一下，这个验证是用来证明这里提交的每一行代码或字符都是绝对可信的(由我本人提交的)，虽不是什么大问题，但不解决的话心里还是很膈应。在网上查阅一番，遇到了一个熟悉的东西——PGP签名，说明git也是通过这种方式来完成验证，先在本地生成公、私密钥，公钥交给云端仓库，自己每次提交的时候用私钥加密。PGP的使用方法以及整个的操作流程在git的 *[官方文档](https://docs.github.com/cn/authentication/managing-commit-signature-verification/checking-for-existing-gpg-keys)* 都有详细的说明，这里就不用赘述。

![git_sign](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/日常琐事/git_sign.png)

**另外提醒一下**，在Windows平台对git完成签名后想要提交时，应该会遇到以下问题：

- 设置签名：

  ```bash
  $ gpg config --global user.signingkey "your_secret_key"
  ```

- 提交一次修改，得到报错：

  ```bash
  $ git commit -S -m "XXX"
  ```

  ```bash
  gpg: skipped "xxx": secret key not available
  gpg: signing failed: No secret key
  ```

明明已经设置好了私钥，那问题出在哪呢？再次网上查阅得知，原来在Windows上还需要告诉git你的`GPG.exe`在哪，这简单，解决了后就可以正常提交了。

- 给git配置GPG路径：

  ```bash
  $ git config --global gpg.program "E:\GnuPG\bin\gpg.exe"
  ```

### AUR网络

在不借助魔法的条件下怎样更新和下载AUR软件，是国内AUR用户的一个痛点。之前我已经在这篇文章 *【[尝试解决AUR下载和更新的问题](https://www.keanu-42.cn/posts/53965/)】* 里讲了方法，可惜没有完成，好在这次在Arch的中文社区里成功解决了这个问题。简单来讲，就是把脚本放在`/usr/bin`目录下就可以了，记得在`makepkg.conf`里添上 **“.sh”** 后缀！虽然我把脚本放在`home`下的时候也是`chmod +x`了的，但就是不行，真是玄学（

> 完整的讨论过程 *[点我](https://bbs.archlinuxcn.org/viewtopic.php?id=12144)*

![aur_without_vpn](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/aur%20%26%20pamac/aur_without_vpn.png)

> <center>不需要魔法的AUR，多是一件美事啊～</center>

讨论的过程中还有另一个收获——***开源社区的语言风格***，关于这方面，替我解决脚本问题的这位Arch社区的大佬是这样说的，我记录下来：

<center>回复</center>

----

> @yw662
> 
> 成为高手的第一步就是，不要因为你现在是一个菜鸟就**心安理得地**接受其他人的帮助。需要> 解决你的问题的人是你自己，能解决这些问题的人也只有你自己。
> 
> **因为其他人没有帮助你就指责其他人**，这不是互助，**把自己放在需要被帮助的立场来等待其> 他人的帮助，而不愿意对自己遇到的问题负责**，这不是互助。
>> 倒也不是想指责什么，我也并不是认为**大佬应该帮助菜鸟**是理所应当，只是那个人的评论有点居高临下的味道。

----

### Linux下的独显驱动

前两天想用OBS录视频的来着，发现软件找不到显示画面，个人第一反应是独显出了问题，用`nvidia-smi`一看，才发现驱动挂了。OBS录不了视频不打紧，关键是驱动这个问题得解决，我当时就在Manjaro社区发了求助贴，可能是中文用户太少的原因，虽有人回复我但并没有给出答案。后面我又重新用英文发了一遍（果然只有这样才能让更多人看到），问题终于解决。之前我系统里的两个内核`linux513`、`linux514`都不支持最新的显卡驱动，所以得卸载了并换成了现在的515，然后把驱动`video-hybrid-nvidia-prime`重新安装一遍就可以了，记得重启（**注销不等于重启**）。讨论过程中我和另一位朋友还提到了**Wayland**，毕竟现在Gnome已经更新为Wayland框架，独显出了问题多多少少和这个有点关系。答案虽看似简单，但实际上是经过我和社区的大佬高强度对线了一整个上午才得出来的（bushi，累归累，但由衷感谢这些给我提供帮助的人，他（她）们都是最可爱的。讨论重点如下：

<center>回复</center>

----

> @omano
> 
> You have unsupported kernels, this is why it is important to not cut down information you give (like you did with `inxi`).
> 
> Install supported kernel.
> 
> `mhwd-kernel -i linux515`
> 
> Reboot, and remove the unsupported kernels
> 
> `mhwd-kernel -r linux513`
> 
> `mhwd-kernel -r linux514`
> 
> Before doing that maybe give full list of kernels used with `mhwd-kernel -li` so we can have a better view. Also a full `inxi -Fazy`

> @omano
> 
> Maybe you could try to remove the `video-hybrid-intel-nvidia-prime` driver from MHWD, and try to reinstall it, or install the 470 driver instead.
> 
> If `inxi` still doesn’t show Nvidia driver loaded for your Nvidia card, make sure you don’t have blacklist somewhere forbidding loading the Nvidia module (optimus manager or bumblebee might do that, you said you don’t have it but that doesn’t mean you didn’t install it at some point).
> 
> For additional info give output of `ls /usr/lib/modprobe.d/` and `ls /etc/modprobe.d/
`
----

> 完整的讨论过程 *[点我](https://forum.manjaro.org/t/nvidia-gpu-not-working-possibly-related-to-latest-driver/103303/)*

通过`inxi -G`命令可以看到，独显使用的是NVIDIA驱动，且系统运行在X11框架上（后面又有一次大更

```bash
Graphics:
  Device-1: Intel WhiskeyLake-U GT2 [UHD Graphics 620] driver: i915 v: kernel
  Device-2: NVIDIA GP108M [GeForce MX250] driver: nvidia v: 510.54
  Device-3: Realtek USB Camera type: USB driver: uvcvideo
  Display: x11 server: X.org v: 1.21.1.3 driver: X:
    loaded: modesetting,nvidia unloaded: nouveau gpu: i915
    resolution: 1920x1080~60Hz
  OpenGL: renderer: Mesa Intel UHD Graphics 620 (WHL GT2)
    v: 4.6 Mesa 21.3.7

```

<center>最后还想说一句：开源社区不好混，你得一直保持严谨的态度。</center>

<center>至此，告一段落...</center>
