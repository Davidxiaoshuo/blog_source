# MacOS Terminal 美化 【程序猿推荐】


![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)

> 工欲善其事，必先利其器

我们在MacOS 下开发的时候不可避免一定为会用的terminal，不管你是server端选手，客户端选手，又或者是...哎呀，等等吧，总之terminal是我们在开发中使用评率比较高的基础开发工具了。但是原生的terminal，是一个及其简陋的家伙。那么怎么能让我们的terminal能够看起来既舒服又能超好用呢。动起手来...


## 先晒一张我的terminal截图：

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/terminal_screenshot.png)

## 改造Terminal步骤

- [安装oh-my-zsh](https://ohmyz.sh)

- 设置 oh-my-zsh [主题](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)

- 安装 oh-my-zsh 日常所需的插件，以提高工作效率

- 修改 Terminal 的Profile，让我们的 Terminal 与 zsh 的主题更加匹配


### 准备工作

首先我们先安装MacOS 中比较好用的软件包管理器 [Homebrew](https://brew.sh)

```

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```

### 安装 oh-my-zsh

```

sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

```

### 配置 oh-my-zsh 主题

首先我们链接到 oh-my-zsh 主题的页面 --> [https://github.com/robbyrussell/oh-my-zsh/wiki/Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)
选择我们喜欢的主题，然后记住主题名字，这里我的主题名字是：**<u>pygmalion</u>**

