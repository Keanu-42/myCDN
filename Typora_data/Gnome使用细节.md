## 目录

- [引言](#a1)
- [输入法](#a2)
- [鼠标滚轮](#a3)
- [开机自启](#a4)
- [科学上网](#a5)
	- [插件管理](#a5-b1)
- [耳机问题](#a6)
- [JAVA环境配置](#a7)
	- [JDK 11](#a7-b1)
		- [JRE](#a7-b1-c1)
- [Android Studio](#a8)
	- [镜像列表及使用方法](#a8-b1)
- [VS Code](#a9) 
- [Adobe在Linux中的替代品](#a10)
- [DaVinci-Resolve](#a11)
	- [Linux查看GPU信息和使用情况](#a11-b1)
- [PGP签名问题](#a12)
	- [在安装密钥之前](#a12-b1)

----

## <span id=a1>引言</span>

作为一个Linux初学者，在第一次接触到[Manjaro（kde）](htts://manjaro.org)时就深深地爱上了这个系统，以至于后来重装系统时选择的也是Manjaro（Gnome），之所以选择后者，是因为Gnome的风格和操作习惯更适合我。这个系统我已经用了有一段时间，有几个比较困扰人，可能大多数人都会遇到的的问题今天一起把它们解决了 ^_^ 。

## <span id=a2>输入法</span>

安装输入法，我这里选用的是`Googlepinyin`。一般现在的系统，在软件管理程序中下载输入法时就包括有相应的依赖包。

![依赖包](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/依赖包.png)

如果没有依赖包或者不全，我们只好手动安装：

```markdown
依赖包
fcitx
fcitx-im
fcitx-cconfigtool
kcm-fictx
libgooglepinyin----或lib（xxxpinyin）
```

输入法和依赖包都安装好后，然后配置环境。我们需要在`/etc/profile`中添加以下内容并关机重启：

```markdown
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

![输入法配置](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/输入法配置.png)

安装好的输入法：

![](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/安装好的输入法.png)

## <span id=a3>鼠标滚轮</span>

新系统装好后可能会遇到滚轮速度过慢的问题，不解决的话真的很影响日常使用。我们先下载`imwheel`这个程序，然后在` ~/.imwheelrc`中添加程序设置（注：在vim中，“Esc”退出，“i”插入），如下：

```markdown
".*"
None,      Up,      Button4, 2
None,      Down,    Button5, 2
Control_L, Up,      Control_L|Button4
Control_L, Down,    Control_L|Button5
Shift_L,   Up,      Shift_L|Button4
Shift_L,   Down,    Shift_L|Button5
None,      Thumb1,  Alt_L|Left
None,      Thumb2,  Alt_L|Right
————————————————
- 第二行和第三行中的“2”代表滚动的行数，可以根据需要来修改这个配置。
- 首行中“.*”用来指定在哪些应用中生效，“.*”表示全部应用生效，可以执行“man imwheel”查看更多帮助信息。
- 第四行和第五行可以让鼠标支持“左ctrl+上下滚动”（比如在浏览器中支持放大）。
- 第六行和第七行可以让鼠标支持“左shift+上下滚动”（不同应用有不同作用），实测似乎并没有什么卵用。
- 最后两行用来开启鼠标侧键功能。
————————————————
版权声明：本文为CSDN博主「15wylu」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_32767041/article/details/84034280
```

## <span id=a4>开机自启</span>

上面的`imwheel`安装好后还要设置开机自启，也有一些其他软件需要设置，比如`IDM`，`Qv2ray`等。有的Linux发行版可以直接在系统设置里面添加，我的Gnome也可以，但是却只能添加完整的应用软件，不能是单独的程序。所以针对Gnome，我们可以在`gnome-session-properties`中设置，如果没有则需要事先下载。
下载好后，打开就是这样。在添加程序时，若不知道安装位置，可以使用`whereis “name”`指令查看。

![设置界面](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/开机自启.png)

## <span id=a5>科学上网</span>

在Linux中如何科学上网这个问题困扰了我许久，解决后才知道如此简单（~~菜是原罪~~）。代理软件我用的是Qv2ray（好用不多说），我们需要下载两个东西，[v2ray-core](https://github.com/v2fly/v2ray-core)和[Qv2ray](https://github.com/Qv2ray/Qv2ray)。

![v2ray-core](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/v2ray-core.png)

![Qv2ray](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/qv2ray-appimage.png)

下载好后，我们先解压v2ray-core，然后打开Qv2ay（若无法打开，则右键属性修改权限，允许执行文件），我们现设置“首选项”，在“内核设置”里，修改“可执行文件路径”和“资源目录”。

“可执行文件路径”，点击右边按钮选择刚刚解压的文件，找到“v2ray”，点击确定。

![文件路径](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/文件路径.png)

而“资源分配”则直接选择总的解压文件，然后在“连接设置”里把“绕过中国大陆IP”勾选上，其它设置就不用管了。

![Qv2ray首选项](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/Qv2ray首选项.png)

因为我个人使用但是机场订阅链接，所以这里主要讲订阅链接的使用方法。

![Qv2ray](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/Qv2ray.png)

首先点击左下角的“分组”，在“组编辑器”里选择“订阅设置”，勾选下方的“此分组是一个订阅”并输入订阅链接，最后点击下方的“更新订阅”就可以确定并退出。

![](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/添加订阅.png)

当然你可以选择手动配置，方法是点击左下角“新建”，选择“手动输入”，再点击下方的“打开连接编辑器”即可手动配置。

双击机场节点，并在设置里选择“系统代理”即可正常使用。

### <span id=a5-b1>插件管理</span>

在Qv2ray的左上角还有一个“插件”选项，点进去后我们可以在这里添加一些常用插件，比如SSR，Trojan，NaiveProxy等插件，可以让我们使用其它格式的订阅链接。

注：插件同样也要赋予可执行权限。

- [QvPlugin-SSR](https://github.com/Qv2ray/QvPlugin-NaiveProxy)
- [QvPlugin-SS](https://github.com/Qv2ray/QvPlugin-SS)
- [QvPlugin-NaiveProxy](https://github.com/Qv2ray/QvPlugin-NaiveProxy)
- [QvPlugin-Trojan](https://github.com/Qv2ray/QvPlugin-Trojan)
- [QvPlugin-Trojan-Go](https://github.com/Qv2ray/QvPlugin-Trojan-Go)

## <span id=a6>耳机问题</span>

其实在第一次使用Manjaro时就遇到耳机（有线）无法识别的问题，但并不影响，因为可以在“音量控制”里找到“Headphones”选项，虽然显示“（已拔出）”但可以正常使用。

这次在Gnome上也遇到这个问题，不过并不是像上面一样，而是根本无法识别出耳机。

![Headphones](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.1/Gnome使用细节/耳机问题/Headhphones.png)

针对这种情况，我们需要下载另一个音量控制程序——“PulseAudio”。

![PulseAudio](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.1/Gnome使用细节/耳机问题/PulseAudio.png)

安装好后，我们在”端口“选项中就可以找到”Headphones“，显示”（已拔出）“但不影响。

![音量控制](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.1/Gnome使用细节/耳机问题/音量控制.png)

现在，可以正常使用你的耳机啦。

**补充**：用命令行修复输出设备无法识别，在终端里输入以下命令后并重启计算机。

```
- echo "options snd-intel-dspcfg dsp_driver=1" > /etc/modprobe.d/alsa.conf
```

## <span id=a7>JAVA环境配置</span>

### <span id=a7-b1>JDK 11</span>

JAVA现在普遍用的版本是11，而最常用的是Oracle家的jdk11，所以我也下载的是这个，[点击下载](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)。

根据下方图片所示完成下载后，我们把压缩包解压到目录`/usr/java`。

![JDK下载](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/jdk11下载.png)

解压好后，我们就可以在`/etc/profile`中修改环境变量，添加以下内容：

```markdown
export JAVA_HOME=/usr/java/xxx-----XXX为你使用的jdk

export JRE_HOME=${JAVA_HOME}/jre

export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH

export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin

export PATH=$PATH:${JAVA_PATH}
```
最后重启电脑以生效。

> 出处：https://www.jianshu.com/p/12493db8a701

#### <span id=a7-b1-c1>JRE</span>

jdk9以后的版本不再自动生成jre，这时我们需要手动生成。如果java目录中没有jre，那么上面环境变量中的jre并不会生效。接下来的操作很简单，我们在终端进入到`/usr/java/XXX`，然后执行以下指令：

```markdown
bin/jlink --module-path jmods --add-modules java.desktop --output jre
```

执行成功后，我们可以看到目录中已经有了jre。

![jre](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/JRE.png)

> 出处：https://blog.csdn.net/y_qc_lookup/article/details/99948136

## <span id=a8>Android Studio</span>

AS可以直接从Arch的仓库中下载安装，十分方便。

![as](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/android%20studio.png)

安装好后，我们在使用AS时遇到的最大问题就是SDK，AVD，以及gradle。但是，这些问题都是可以通过科学上网直接解决的，或者使用国内镜像代理。

### <span id=a8-b1>镜像列表及使用方法</span>


- 镜像列表

    <details>
     <summary>南阳理工学院镜像服务器地址：</summary>
     <ul>
      <li>mirror.nyist.edu.cn 端口:80</il>
     </ul>
    </details>

    <details>
     <summary>中国科学院开源协会镜像站地址：</summary>
     <ul>
      <li>IPV4/IPV6: mirrors.opencas.cn 端口:80</li>
      <li>IPV4/IPV6: mirrors.opencas.org 端口:80</li>
      <li>IPV4/IPV6: mirrors.opencas.ac.cn 端口:80</li>
     </ul>
    </details>

    <details>
     <summary>上海GDG镜像服务器地址：</summary>
     <ul>
      <li>IPV4/IPV6: mirrors.opencas.ac.cn 端口:80</li>
     </ul>
    </details>

    <details>
     <summary>北京化工大学镜像服务器地址：</summary>
     <ul>
      <li>IPv4: ubuntu.buct.edu.cn/ 端口:80</li>
      <li>IPv4: ubuntu.buct.cn/ 端口:80</li>
      <li>IPv6: ubuntu.buct6.edu.cn/ 端口:80</li>
     </ul>
    </details>

    <details>
     <summary>大连东软信息学院镜像服务器地址：</summary>
     <ul>
      <li>mirrors.neusoft.edu.cn 端口:80</li>
     </ul>
    </details>

    <details>
     <summary>腾讯Bugly镜像：</summary>
     <ul>
      <li>android-mirror.bugly.qq.com 端口:8080</li>
      <li>腾讯镜像使用方法：http://android-mirror.bugly.qq.com:8080/include/usage.html</li>
     </ul>
    </details>


- 使用方法

    1. 启动Android Studio，打开主界面，依次选择“Configure”，“System Settings”，弹出“HTTP Proxy”窗口；
    2. 在“HTTP Proxy”窗口中，选择“Manual Proxy Configuration”，勾选“HTTP”，在“Host name”和“Port number”输入框内填入上面镜像服务器地址（**不包含http:// **，如下图）和端口，设置完成后单击“Check connection”按钮，最后关闭设置窗口返回到主界面；
    3. 依次选择“Packages”，“Reload”。

![configure](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/configure.png)

![http settings](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/http-settings.png)

> 出处：https://www.cnblogs.com/pingxin/p/p00078.html

## <span id=a9>VS Code</span>

最开始我并不知道Arch的仓库中有[VS Code](https://aur.archlinux.org/packages/visual-studio-code-bin)，我是根据网上的这篇[帖子](https://www.cnblogs.com/kwinwei/p/13202174.html)完成安装，虽然有点多此一举，但是也给其它Linux发行版提供了参考，下面是安装流程：

首先我们去[官网](https://code.visualstudio.com/download)下载最新的VS Code，下载时注意你所选择的文件格式以及架构。

![download vs](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/download%20VS.png)

下载完后，我们把压缩包解压到文件目录`/usr/vs_code`，此时双击“code”就已经可以运行了（可能需要给予执行权限），但是我们还是得创建一个快捷方式。

在程序根目录下，我们可以在`/resources/app/resources/linux/`这个路径中找到VS的图标“code.png”，然后把这个图标复制到`/usr/share/icons`目录下，由于在文件夹中无法直接执行这一操作，所以我们在终端中用以下指令：

```markdown
sudo cp /usr/vs_code/VSCode-linux-x64/resources/app/resources/linux/code.png /usr/share/icons/
```

最后我们就可以在`/usr/share/applications/`目录下创建VS的快捷方式。在终端中，用编辑器创建`/usr/share/applications/VSCode.desktop`，然后在文本框中输入以下内容：

```markdown
[Desktop Entry]
Name=Visual Studio Code
Comment=Multi-platform code editor for Linux
Exec=/usr/local/VSCode-linux-x64/code
Icon=/usr/share/icons/code.png
Type=Application
StartupNotify=true
Categories=TextEditor;Development;Utility;
MimeType=text/plain;
```

保存后退出，我们在应用菜单里就可以看到VS Code了。打开VS Code，加载插件：vscode-icons。

![插件](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/icons.png)

## <span id=a10>Adobe在Linux中的替代品</span>

Photoshop
- GIMP: https://www.gimp.org/
- Krita: https://krita.org/en/
- Photopea: https://www.photopea.com/

Lightroom
- Darktable: https://www.darktable.org/
- RawTherapee: https://rawtherapee.com/

Illustrator
- Inkscape: https://inkscape.org/
- Vectr: https://vectr.com/

InDesign
- Scribus: https://www.scribus.net/
- Lucidoress: https://www.lucidpress.com/pages/

Premiere Pro
- Blender: https://www.blender.org/

After Effects
- Natron: https://natrongithub.github.io/

Audition
- Audacity: https://www.audacityteam.org/

Dreamweaver
- BlueGriffon: http://bluegriffon.org/

## <span id=a11>DaVinci-Resolve</span>

> 让DaVinci-Resolve在Manjaro中跑起来 (abandoned)

今天打算在电脑上装达芬奇，原本是已经有了一个Kdenlive，但是我在看了油管博主@TechHut的视频[[Why I Switched To Resolve from Kdenlive - Best Video Editor on Linux?](https://youtu.be/O1ly_hp3Y-M)]后，对达芬奇产生了浓厚的兴趣，所以就有了后面的一系列操作，视频中其实也提到了从安装到使用的这整个过程并不是很顺利（伏笔）。

老样子，我们还是先去[官网](https://www.blackmagicdesign.com/products/davinciresolve/#)下载镜像文件（~~app store也有，放在最后尝试~~），软件我们选择davinci-resolve 16，下载时选择Linux版本，这时会弹出来一个登记表，随便填一下，点击右下角注册并下载。

![davinci_website](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.2/Gnome使用细节/davinci-resolve/davinci_website.png)

压缩包下载好后，里面分别是安装指导PDF和安装程序，注意，安装程序是`.run`后缀。文件提取出来后不要急着点击安装，我们还需要把DaVinci所需的各种依赖包装上。当然，你也可以先安装，安装好后再检查所缺文件。安装界面和win差不多，一直next就完事[doge]。

![安装界面](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.2/Gnome使用细节/davinci-resolve/安装界面.png)
> 因为我已经安装过了，所以这里的选项是“重装”或“卸载”

DaVinci安装好后，这时一般无法直接启动，因为还缺少依赖，所以接下来我们要检查到底缺哪些东西。

安装目录一般默认是`opt/resolve/`，而程序就在bin文件夹里。我们可以先试试命令`./resolve`启动DaVinci，结果报错，提示如下：

```markdown
error while loading shared libraries: libOpenCL.so.1: cannot open shared object file: No such file or directory
```

通过提示可以明显看出是缺少依赖的，如果想要知道得更清楚，可以使用`ldd resolve`来检查。
这时你就会看到一长串依赖目录，注意每一行的`=>`符号，如果缺少依赖则右边会显示`not found`，正常情况和下面一样：

```markdown
linux-vdso.so.1 (0x00007ffd23fce000)
	libc++.so.1 => /opt/resolve/bin/./../libs/libc++.so.1 (0x00007f6b2d736000)
	libc++abi.so.1 => /opt/resolve/bin/./../libs/libc++abi.so.1 (0x00007f6b2d507000)
	libcudart.so.11.0 => /opt/resolve/bin/./../libs/libcudart.so.11.0 (0x00007f6b2d289000)
	libcublas.so.11 => /opt/resolve/bin/./../libs/libcublas.so.11 (0x00007f6b27437000)
	libnvrtc.so.11.0 => /opt/resolve/bin/./../libs/libnvrtc.so.11.0 (0x00007f6b25c4c000)
	libQt5Concurrent.so.5 => /opt/resolve/bin/./../libs/libQt5Concurrent.so.5 (0x00007f6b25c46000)
	libQt5Core.so.5 => /opt/resolve/bin/./../libs/libQt5Core.so.5 (0x00007f6b25666000)
	libQt5Gui.so.5 => /opt/resolve/bin/./../libs/libQt5Gui.so.5 (0x00007f6b250ec000)
	libQt5Multimedia.so.5 => /opt/resolve/bin/./../libs/libQt5Multimedia.so.5 (0x00007f6b25008000)
	libQt5Network.so.5 => /opt/resolve/bin/./../libs/libQt5Network.so.5 (0x00007f6b24eb5000)
	libQt5OpenGL.so.5 => /opt/resolve/bin/./../libs/libQt5OpenGL.so.5 (0x00007f6b24e50000)
	libQt5Sql.so.5 => /opt/resolve/bin/./../libs/libQt5Sql.so.5 (0x00007f6b24d09000)
	libQt5Svg.so.5 => /opt/resolve/bin/./../libs/libQt5Svg.so.5 (0x00007f6b24cb0000)
	libQt5Widgets.so.5 => /opt/resolve/bin/./../libs/libQt5Widgets.so.5 (0x00007f6b24632000)
	libQt5Xml.so.5 => /opt/resolve/bin/./../libs/libQt5Xml.so.5 (0x00007f6b245e9000)
	libQt5XmlPatterns.so.5 => /opt/resolve/bin/./../libs/libQt5XmlPatterns.so.5 (0x00007f6b24155000)
```

那么现在我们就开始安装依赖包，总共需要安装以下这几类文件：
> 详情请参考B站视频「[好跪好我不是Tim...](https://www.bilibili.com/video/BV1pT4y157CS?share_source=copy_web)」

### ssl

- Arch
	- openssl
	- openssl-1.0
	- lib32-openssl
- Debian / Ubuntu / etc
	- libssl-1.0

### ocl-icd

- Arch
	- lib32-ocl-icd
	- ocl-icd
- Debian / Ubuntu / etc
	- ocl-icd-opencl-dev

### cuda

> 不一定会用上

- Arch
	- cuda
- Debian
	- nvidia-cuda-dev

### opencl

- Arch
	- NVIDIA
		- opencl-headers
		- opencl-nvidia
		- lib32-opencl-nvidia
	- AMD / Intel核显
		- lib32-opencl-mesa
		- opencl-headers
		- opencl-mesa
- Debian / Ubuntu / etc
	- nvidia-opencl-icd

### fuseiso

因为安装程序是`.run`文件，所以需要fuseiso把它内部的镜像文件挂载成一个虚拟文件系统，从里面拷贝文件，否则因挂载失败而导致安装失败（我用的Manjaro不需要），最后记得重启。
> fuseiso是用于arch，其它发行版就需要另外查找

到这里就可以正常启动 / 安装。如果在启动过程中遇到了问题，那么可以查看运行日志，位置就在`opt/resolve/logs/`，在log中我们可以看到有两个error，我个人猜测问题还是出在显卡或驱动上。

```markdown
| ERROR | 2021-08-21 13:22:11,602 | Failed to connect to panel socket
| ERROR | 2021-08-21 13:22:11,602 | DRIVER: open /var/tmp/davinci_socket failed
```

但是前面我已经把各种需要的或不需要的驱动都装上了，而我的笔记本是IU + N卡，那么问题可能在于显卡运行模式（DaVinci对显卡这方面很挑剔）。后面我了解到`optimus-manager`可以控制并切换显卡的运行模式，下载这个软件时，还得再下载一个`optimus-manager-qt`，然后重启电脑。
> 详情请参考B站视频「[超简单Linux双显卡...](https://www.bilibili.com/video/BV1vK4y187Ww?share_source=copy_web)」

当我从终端启动optimus-manager时，结果又报错，看到提示我真的是“黑人问号？？”

![optimus-manager](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.2/Gnome使用细节/davinci-resolve/optimus-manager.png)

提示的第三行说道“If your login manager is GDM, make sure to follow those instructions”，我一看到这个GDM就知道事情不简单，下面提供的[解决方案](https://github.com/Askannz/optimus-manager#important--gnome-and-gdm-users)我点进去一看，GDM指的是“Gnome Display Manager”，然后下面给了两条解决措施，一是替换**gdm**为**gdm-prime**；二是optimus-manager与我的wayland窗口系统不兼容，还得修改`/etc/gdm/custom.conf`，把`#WaylandEnable=false`里的注释取消。

![GDM](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.2/Gnome使用细节/davinci-resolve/GDM.png)

但是在这里我犹豫了，先不说这样做对系统的稳定性会不会产生影响，关键是我也不清楚我的方向是否正确。所以我先暂时放下这个，想去看看独显的运作情况。

### <span id=a11-b1>[Linux查看GPU信息和使用情况](https://www.cnblogs.com/yuehouse/p/10242942.html)</span>

1. 查看核显

	`lspci | grep -i vga`
	
	```markdown
	00:02.0 VGA compatible controller: Intel Corporation WhiskeyLake-U GT2 [UHD Graphics 620]
	```

	上面的`00:02.0`就是核显的代号，利用这个代号输入`lspci -v -s 00:02.0`
	
	<details open>
	 <summary>核显信息</summary>
	 <dl>
	  <dt>00:02.0 VGA compatible controller: Intel Corporation WhiskeyLake-U GT2 [UHD Graphics 620] (prog-if 00 [VGA controller])</dt>
	  <dd>DeviceName: Onboard - Video</dd>
	  <dd>Subsystem: Intel Corporation Device 2112</dd>
	  <dd>Flags: bus master, fast devsel, latency 0, IRQ 127</dd>
	  <dd>Memory at 6012000000 (64-bit, non-prefetchable) [size=16M]</dd>
	  <dd>Memory at 4000000000 (64-bit, prefetchable) [size=256M]</dd>
	  <dd>I/O ports at 5000 [size=64]</dd>
	  <dd>Expansion ROM at 000c0000 [virtual] [disabled] [size=128K]</dd>
	  <dd>Capabilities: access denied </dd>
	  <dd>Kernel driver in use: i915</dd>
	  <dd>Kernel modules: i915</dd>
	 </dl>
	</details>

2. 查看独显

	`lspci | grep -i -nvidia`
	
	```markdown
	01:00.0 3D controller: NVIDIA Corporation GP108M [GeForce MX250] (rev a1)
	```
	
	上面的`01:00.0`就是独显的代号，利用这个代号输入`lspci -v -s 01:00.0`
	
	<details open>
	 <summary>独显信息</summary>
	 <dl>
	  <dt>01:00.0 3D controller: NVIDIA Corporation GP108M [GeForce MX250] (rev a1)</dt>
	  <dd>Subsystem: Device 1b50:5515</dd>
	  <dd>Flags: bus master, fast devsel, latency 0, IRQ 137</dd>
	  <dd>Memory at 80000000 (32-bit, non-prefetchable) [size=16M]</dd>
	  <dd>Memory at 6000000000 (64-bit, prefetchable) [size=256M]</dd>
	  <dd>Memory at 6010000000 (64-bit, prefetchable) [size=32M]</dd>
	  <dd>I/O ports at 4000 [size=128]</dd>
	  <dd>Expansion ROM at 81000000 [virtual] [disabled] [size=512K]</dd>
	  <dd>Capabilities: access denied</dd>
	  <dd>Kernel driver in use: nvidia</dd>
	  <dd>Kernel modules: nouveau, nvidia_drm, nvidia</dd>
	 </dl>
	</details>

3. 利用`nvidia-smi`查看独显

	```markdown
	+-----------------------------------------------------------------------------+
	| NVIDIA-SMI 470.63.01    Driver Version: 470.63.01    CUDA Version: 11.4     |
	|-------------------------------+----------------------+----------------------+
	| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
	| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
	|                               |                      |               MIG M. |
	|===============================+======================+======================|
	|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 Off |                  N/A |
	| N/A   42C    P8    N/A /  N/A |      4MiB /  2002MiB |      0%      Default |
	|                               |                      |                  N/A |
	+-------------------------------+----------------------+----------------------+
	| Processes:                                                                  |
	|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
	|        ID   ID                                                   Usage      |
	|=============================================================================|
	|    0   N/A  N/A      1320      G   /usr/lib/Xorg                       4MiB |
	+-----------------------------------------------------------------------------+
	```
	
	看到这个后我迷惑了，这独显有在工作吗？
	行吧，不想折腾了，app-store我也试过了，无法下载，解决方法应该是把之前下好的压缩包放在一个下载目录，但是这样做也没什么意义（虽然我也没找到），最后还是会回到启动报错的问题。
	Kdenlive又不是不能用。

## <span id = a12>PGP签名问题</span>

### Describe the bug

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

### To Reproduce

- 根据pjbrunet的描述，我在`var/lib/pacman/sync`该路径下看到了sig文件并把它们删掉，pacman可以正常使用了，但pamac并没有。我再次使用pamac更新软件时，该路径下依然会产生‘.sig’文件，同样的操作在手机热点下结果一样，所以我并不确定是我的网络问题还是pamac的问题。

![d_sigs](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/d_sigs.png)

----

![pacman_work](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/pacman_work.png)

- According to pjbrunet, I saw the '.sig' files in `var/lib/pacman/sync` and deleted them , pacman works fine but pamac  does not. When I used pamac to upgrating the software again, the '.sig'files was still generated in this path. The same operation had the same result with my  mobile's wifi , so I wasn't sure if it was my network or pamac .

### System information

 - OS: Manjaro Gnome 40
 - pamac version: 10.1.3-3
 - Pacman: v6.0.0 - libalpm v13.0.0

### Additional context

- network: campus network
- mirrorlist: global (refresh frequently)

### Solutions

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

### <span id = a12-b1>在安装密钥之前</span>

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

----

> 后续若有其它内容，我再一一补充 ^_^ 。

<center>注：本文部分内容整理自互联网，文中已标明出处。</center>