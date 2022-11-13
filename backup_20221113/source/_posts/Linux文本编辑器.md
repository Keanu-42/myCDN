---
title: Linux文本编辑器
tags:
  - vim
  - spacevim
categories:
  - 杂谈
cover: >-
  https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/日常琐事/vim_awesome.png
abbrlink: 33734
date: 2022-05-05 15:49:03
---
## <center>引言</center>

如果你问我在Linux上最常用、最喜欢的编辑器是什么，那我肯定回答是 *“[Vim](https://baike.baidu.com/item/VIM/60410)”* 。

## <center>正文</center>

“vim / vi”的上手门槛挺高，新手第一次接触这种编辑器不知道怎么使用是很正常的，但是习惯了之后，在简单编辑一些文本这方面是真的不想再用其它的了。我本人就很喜欢vim的“模式”这种概念或使用逻辑。

或许你已经看过很多编程大佬把他们的vim配置得非常炫酷和实用，也很惊叹他们使用vim时键步如飞的操作，但是切忌不要盲目跟风，这不见得适合你,我使用vim的另一个原因就是喜欢它的的简洁。

![vim](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/windows_terminal/vim.png)

我自己在使用vim的时候就只用了两三个插件， *“[nerdtree](https://github.com/preservim/nerdtree)”* 、 *“[vim-dict](https://github.com/skywind3000/vim-dict)”* 和 *“[vim-auto-popmenu](https://github.com/skywind3000/vim-auto-popmenu)”* ，前者都知道，后者分别是字典和自动补全。在安装这些插件前得先安装一个 *“[vim-plug](https://github.com/junegunn/vim-plug)”* ，这事专门用来管理插件的，很实用。而且网上还有个专门收录vim插件的网站叫 *“[VimAwesome](https://vimawesome.com/)”* 。

### 安装方法：

- vim

    ```bash
    $ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    ```

- nvim

    ```bash
    $ sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
    ```

### 这是我的配置：

`~/.config/nvim/init.toml`

```bash
call plug#begin()

Plug 'scrooloose/nerdtree'

Plug 'skywind3000/vim-dict'

let g:vim_dict_config = {'html':'html,javascript,css', 'markdown':'text'}

Plug 'skywind3000/vim-auto-popmenu'

" 设定需要生效的文件类型，如果是 "*" 的话，代表所有类型
let g:apc_enable_ft = {'text':1, 'markdown':1, 'php':1}

" 设定从字典文件以及当前打开的文件里收集补全单词，详情看 ':help cpt'
set cpt=.,k,w,b

" 不要自动选中第一个选项。
set completeopt=menu,menuone,noselect

" 禁止在下方显示一些啰嗦的提示
set shortmess+=c

call plug#end()

```

### 使用技巧

怎样在vim里使用系统剪贴板是个老生常谈的问题，首先你得看你的vim支不支持“clipboard”，可以输入以下命令查看，如果“clipboard”前有个“+”符号则支持。

```bash
$ vim --version | grep "clipboard"
```

如果不支持的话，你可以先卸载当前vim然后安装gvim，这样一同安装的vim就支持剪贴板同步。

> neovim是真的不支持

- 复制vim里的内容到剪贴板

    在“可视模式”下，选中内容输入 `" + y` 完成复制

- 粘贴剪贴板里的内容到vim

    在“普通模式”下，按快捷键 `" + p` 完成粘贴

### SpaceVim

如果你确实想在vim里打造一个编程环境，那么这里有一个现成的方案—— *“[SpaceVim](https://spacevim.org/cn)”* 。好巧的是，这个项目的作者是国人，所以你不用担心官方文档看不懂，并且该项目在github上已收获18k star。“spacevim”给你提供了一个现成的插件环境，你只需要按照自己的需求并结合官方文档就可以把 vim / nvim 打造成你所需要的样子。我自己在Windows上用过一段时间，可惜受系统所限，spacevim的相应速度差强人意。

![spacevim_win](https://cdn.jsdelivr.net/gh/Keanu-42/myCDN@main/windows_terminal/spacevim_win.png)

在终端里我更偏向于原本的vim，而要编程的话我推荐 *“[Doom Emacs](https://github.com/doomemacs/doomemacs)”* ，这样可以很好的分开使用，前提是你是用“记事本”编程的人（
