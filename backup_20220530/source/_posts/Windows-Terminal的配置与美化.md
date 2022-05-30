---
title: Windows Terminal的配置与美化
tags:
  - Windows
  - terminal
categeries:
  - 教程
  - Windows
cover: >-
  https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/windows_terminal/winfetch.png
abbrlink: 45704
date: 2021-11-09 19:33:00
---
![myWindowsTerminal](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/windows_terminal/new_powershell.jpg)
## 前言

***最近我把我的电脑更新到win11了，短短使用了几天后，在系统动画和UI这些方面win11都让我挺满意的，bug也基本没有遇到（~~自定义分辨率导致设置窗口闪退~~），Windows Terminal也从开发版过渡到稳定版，而这也无疑是我在windows上最喜欢的工具之一，写这篇帖子就是来简单地聊聊我的windows终端的使用经验。***

----

## 配置

对于首次接触或大概知道Windows Terminal的人来说，这个可能就是一个窗口管理器，不就是把powershell和cmd集中起来并换了个窗口嘛，我当时也是这样想，但是当我打开设置的json文件时，我知道了这个的自定义功能非常强，而且我也被它的窗口切分这个功能所深深吸引（没见过世面）。配置这方面倒也不复杂，不论是主题样式、快捷键还是添加新的应用程序，都给了相应的模板，只需要你依葫芦画瓢。而现在的windows terminal已经有了完整的设置界面，也不需要你一定要从文本里修改了（当然也可以）。

![settings.json](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/settings_json.jpg)

打开设置时，在第一行有一个“启动”，在这里我们有两个设置要更改，一个是“默认配置文件”，这个决定当你打开Windows Terminal时的默认应用程序；第二个是“默认终端应用程序”，在这里，如果你选择Windows Terminal，那么，当你使用<kbd>Win</kbd>+<kbd>R</kbd>运行CMD或Powershell时，都会以Terminal的形式打开。

![启动](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/启动.jpg)

### powershell

powershell这个大家都不陌生，但是你可能不知道的是windows自带的这个版本其实已经很旧了，你要做的只是从微软商店或他们的[git仓库](https://github.com/microsoft/terminal)下载最新的版本就行，安装好后，打开windows terminal你就能在选项卡里看到新的powershell了，没错，新旧并存。

![store_powershell](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/store_powershell.jpg)

有了新的powershell后，旧的咱也用不上，不用删掉只需隐藏即可。怎么做呢？打开设置界面，点击左下角的“打开JSON文件”，找到name为“Windows Terminal”的配置文件，或使用<kbd>Ctrl</kbd>+<kbd>F</kbd>快速定位。找到“hidden”这一行，把冒号后的“false”改为“true”就可以了，保存修改并重新打开terminal，在选项卡的下拉菜单里就看不到了旧的powershell。如果你还想把每个子程序的上下顺序换一下时，只需在JSON文件里把他们的配置文件顺序改一下就行。

![选项卡下拉菜单](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/选项卡.jpg)

### winget

winget大家应该比较陌生，这个其实就相当于包管理器，和Linux下的apt、dpkg、以及pacman差不多，是不是很意外？是的，现在你在windows上也能用包管理器了。但是，如果你发现你在win10上用不了这个命令，那是正常的，你得更新一下Windows的appinstaller，在微软商店里搜索“应用安装程序”即可更新（不可关闭Windows的系统更新）。

***注：winget 命令行工具仅在 Windows 10 1709（版本 16299）或更高版本上受支持。***

![winget](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/winget.jpg)

winget的用法和其它包管理器类似，很简单，例如：

***注：使用winget命令时，需全程科学上网***

```bash
//搜索应用程序
winget search <package name>

//根据ID搜索应用程序
winget search --id <package name>

//安装应用程序
winget install <package name>

//根据ID安装应用程序
winget install -id <package name>

//显示应用程序的详细信息
winget show <package name>
...
```

> *更多有关winget的内容详见微软的[官方文档](https://docs.microsoft.com/zh-cn/windows/package-manager/winget/)*

### WSL

WSL（Windows Subsystem for Linux），即适用于Linux的Windows子系统，可以在Windows上构建出一个原生的Linux环境。WSL不仅是一个名字，同时也是用来下载、安装和管理Windows子系统的命令工具，你可以在powershell中键入`wsl --help`以查看基本命令。

目前微软官方提供了以下几个Linux发行版的子系统，你可以在微软商店里安装，或使用WSL命令工具安装。

```bash
NAME            FRIENDLY NAME
Ubuntu          Ubuntu
Debian          Debian GNU/Linux
kali-linux      Kali Linux Rolling
openSUSE-42     openSUSE Leap 42
SLES-12         SUSE Linux Enterprise Server v12
Ubuntu-16.04    Ubuntu 16.04 LTS
Ubuntu-18.04    Ubuntu 18.04 LTS
Ubuntu-20.04    Ubuntu 20.04 LTS
```

**一些常用的WSL命令：**

- `wsl -l -o`：查看可供下载的Windows子系统。
- `wsl -l -v`：查看子系统所使用WSL版本（1或2）
- `wsl --set-version`：设置指定运行Linux发行版的WSL版本（1或2）
- `wsl --set-default-version`：设置默认WSL版本
- `wsl --set-default`：设置默认Linux发行版

我个人目前在电脑上安装了Ubuntu-18、Ubuntu-20和Debian，使用体验和在完整的Linux上没有什么区别，除了没有桌面/GUI。但是，现在WSL2支持安装GUI软件，安装好后，你可以直接在开始菜单里直接找到并打开，就像windows上的应用一样，微软真不愧是最佳Linux开发者。

> *更多有关WSL的内容详见微软的[官方文档](https://docs.microsoft.com/zh-cn/windows/wsl/about)*

----

## 美化

放在文章开头的那张图就是我已经美化好了的Windows Terminal，看着还不错吧。总的来说我就做了三件事：更改主题、安装字体、修改终端样式。

### 更改主题

更改主题很简单，在设置里看到“配置文件”，下方有“默认值”和你已经安装了的Powershell、CMD和WSL。默认值相当于全局配置，你在这里做的修改对所有子程序生效，或者你也可以选择单独修改，而我选择的是修改默认值。在“常规”这里，你可以修改字体样式和背景，我的设置是：

![外观](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/外观.jpg)

----

| 选项     |     配置     |
| :------: | :--------------:|
| 文本颜色 |      MonaLisa    |
| 字体大小 |        13        |
| 字体样式 | CaskaydiaCove NF |
|   背景   |   亚克力：40%    |

----

### 安装字体

之所以要安装字体，是因为在安装主题插件时，有些特殊字符显示不了，不过换一种字体就可以了。我这选择的是“Caskaydia”，你可以在[Nerd Fonts官网](https://www.nerdfonts.com/)下载。安装字体的方法也很简单，选中你要安装的字体，把它们拖进Windows的字体目录就行了，具体是：`C:\Windows\Fonts`。

> 这个也同样解决了我在WSL里使用“oh-my-zsh”时，样式错乱的问题

### 修改终端样式

修改Powershell的终端样式也是和Linux一样，安装第三方插件，不过不叫“oh-my-zsh”，而是“oh-my-posh”，oh-my-posh也可以用在Linux终端上。

- 官方网站：https://ohmyposh.dev/
- git仓库：https://github.com/jandedobbeleer/oh-my-posh

然而我并没有在git上下载，而是通过winget，活学活用。

![winget_ohmyposh](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/ohmyposh.jpg)

> *小tip：在终端里，只需右键单击即可快速完成复制和粘贴*

![gif](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/terminal.gif)

> *ScreenToGif是一个很好用的录制GIF的小工具*

安装好后，在终端里输入`ohmyposh.exe`，如果输出终端样式发生改变则安装成功。然后继续输入`notepad $PROFILE`打开你的配置文件，第一次打开时可能会提示该文件并不存在，问你是否创建，点击“是”就行了。打开后，根据[官方文档](https://ohmyposh.dev/docs/windows)的要求，我们要在文本框里输入以下指令：

```bash
oh-my-posh --init --shell pwsh --config ~/jandedobbeleer.omp.json | Invoke-Expression
```
***注：“~”要替换为你的主题文件所在目录，“oh-my-posh”前也建议加上路径，后面的JSON文件就是你的默认主题，而oh-my-posh默认安装了大量主题，你可以自行选择***

这里给出我的配置，以供参考：

```bash
C:\Users\FallingSky\AppData\Local\Programs\oh-my-posh\bin\oh-my-posh --init --shell pwsh --config 

C:\Users\FallingSky\AppData\Local\Programs\oh-my-posh\themes\jandedobbeleer.omp.json | Invoke-Expression
```

----

### 窗口拆分

- 垂直拆分：<kbd>Alt</kbd>+<kbd>Shift</kbd>+<kbd>+</kbd>
- 水平拆分：<kbd>Alt</kbd>+<kbd>Shift</kbd>+<kbd>-</kbd>
- 关闭窗口：<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>W</kbd>

![windows](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.6/windows_terminal/拆分窗口.jpg)

做到这里，你就拥有了一个简单美观，易配置的Windows Terminal了。

>　终端里的黑色小三角估计也是字体原因

----

### 补充

当你看到封面里的终端图片和文章里的差别那么大时，请不要奇怪，因为这就是我后来重新修改的样子: )

#### 字体

字体我重新选用的是`Hack NF`，依然还是在[Nerd Fonts](https://nerdfonts.com)网站下载。这个字体更圆润、更粗一些，看着更舒服。

#### 背景

背景我重新添加了一个颜色配置文件，操作很简单，只需复制一份`One Half Dark`的配置并重命名，然后修改`background`的数值为`#001B26`即可。除此之外，在Terminal的外观设置当中，可以把“在选项卡行中显示亚克力效果”打开，这样窗口边框也有了透明效果。

#### 主题推荐

- powerlevel10k_rainbow.omp.json

  这个主题是`oh-my-posh`自带的，且样式不存在黑色小三角的问题，双行显示也给你留下了足够的打字空间，不用担心主题样式过长: ) 。

  ![new1_pwsh](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/windows_terminal/new1_pwsh.png)

- [takuya.omp.json](https://github.com/craftzdog/dotfiles-public/tree/master/.config/powershell)
  
  这是一个第三方主题，来自于一位我很喜欢的油管博主[devaslife](https://www.youtube.com/c/devaslife)，人如其名是一个程序员大佬，他个人打造的pwsh、fish shell、vim等，都非常美观实用。在这个主题中，你可以按住`ctrl`并点击鼠标左键可以打开当前所在路径，非常方便。

  ![pwsh_takuya](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/windows_terminal/pwsh_takuya.png)

- 插件：[winfetch](https://github.com/kiedtl/winfetch)

  winfetch就是类似于screenfetch、neofetch用来显示系统信息的小程序，可以通过Windows平台的包管理器[PSGallery](https://www.powershellgallery.com/)、[Scoop](https://scoop.sh/)和[Chocolatey](https://chocolatey.org/)来安装（不包括[Winget](https://docs.microsoft.com/zh-cn/windows/package-manager/winget/)）。

  ![winfetch](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/windows_terminal/winfetch.png)
  
  PSGallery

  ```bash
  $ Install-Script -Name pwshfetch-test-1
  
  $ Set-Alias winfetch pwshfetch-test-1    # Put this in $PROFILE
  ```

  Scoop

  ```bash
  $ scoop install winfetch
  ```

  Chocolatey

  ```bash
  $ choco install winfetch -y
  ```

  通过pwsh手动安装（管理员模式）

  ```bash
  # download development version
  $ Invoke-WebRequest "https://raw.githubusercontent.com/kiedtl/winfetch/master/winfetch.ps1" -OutFile ~\.local\bin\winfetch.ps1 -UseBasicParsing

  # download a specific version
  $ Invoke-WebRequest "https://raw.githubusercontent.com/kiedtl/winfetch/v2.0.0/winfetch.ps1" -OutFile ~\.local\bin\winfetch.ps1 -UseBasicParsing
  ```
