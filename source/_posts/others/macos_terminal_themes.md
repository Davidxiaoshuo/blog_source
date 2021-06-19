---
title: MacOS Terminal 美化 【程序猿推荐】
categories: Other
tags: other
---

# MacOS Terminal 美化 【程序猿推荐】

![](https://travis-ci.org/joemccann/dillinger.svg?branch=master)

> 工欲善其事，必先利其器

我们在MacOS 下开发的时候不可避免一定为会用的terminal，不管你是server端选手，客户端选手，又或者是...哎呀，等等吧，总之terminal是我们在开发中使用评率比较高的基础开发工具了。但是原生的terminal，是一个及其简陋的家伙。那么怎么能让我们的terminal能够看起来既舒服又能超好用呢。动起手来...


## 先晒一张我的terminal截图：

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/terminal_screenshot.png)

## 改造Terminal步骤

- [安装oh-my-zsh](https://ohmyz.sh)

- 设置 oh-my-zsh [主题](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)

- 安装 oh-my-zsh 日常所需的插件，以提高工作效率

- 修改 Terminal 的Profile，让我们的 Terminal 与 zsh 的主题更加匹配


## 准备工作(可能会需要)

首先我们先安装MacOS 中比较好用的软件包管理器 [Homebrew](https://brew.sh)

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```

## 安装 oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## 配置 oh-my-zsh 主题

首先我们链接到 oh-my-zsh 主题的页面 --> [https://github.com/robbyrussell/oh-my-zsh/wiki/Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)
选择我们喜欢的主题，然后记住主题名字，这里我的主题名字是：**<u>pygmalion</u>**

然后我们打开Terminal窗口输入以下命令对 .zshrc 文件进行编辑

```bash
vim ~/.zshrc

// 将 ZSH_THEME 设置为即将要是用的主题名称
ZSH_THEME="pygmalion"

```

![如图](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/zshrc_screenshot.png)

设置成功后，退出vim。执行以下命令，使刚刚的设置生效：

```bash
source ~/.zshrc
```

如果你细心你会发现实际上oh-my-zsh在安装的时候已经内置了很多的主题。

```bash
cd ~/.oh-my-zsh/themes // 主题目录
cd ~/.oh-my-zsh/plugins // 插件所在目录
```

## 安装oh-my-zsh插件

oh-my-zsh 的插件有很多，它本身内置的插件也有很多，但是我们在用的时候可以根据需要设置需要的即可。***因为插件安装过多，一定程度上会使zsh的命令执行效率变低***

## zsh-syntax-highlighting

zsh 语法高亮插件，[官方地址](https://github.com/zsh-users/zsh-syntax-highlighting)

```bash
vim ~/.zshrc

//添加如下脚本
plugins=(
    git
    zsh-syntax-highlighting
)
```

安装效果如下：

![](https://raw.githubusercontent.com/zsh-users/zsh-syntax-highlighting/master/images/after2.png)

## 修改 Terminal 的Profile，导入我们想要的主题 【我选择的是***Fideloper***】

[macos-terminal-themes](https://github.com/lysyi3m/macos-terminal-themes) 提供了丰富的主题供我们选择，我们可以在当中找一款与我们所选zsh主题相符合的主题。

- 将 macos-terminal-themes 下载到本地
- 进入到 schemes 目录下
- 双击 *.terminal 的文件，将会打开一个所选主题的新的Terminal窗口
- 选择Terminal 工具栏中的 Shell -> Use Setting as Default, 即将主题修改为所选主题。


