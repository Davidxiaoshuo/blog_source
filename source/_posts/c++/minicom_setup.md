---
title: USB 转串口工具 minicom Setup
tags: c++
---

### 准备工作

1. 安装 minicom 串口调试工具, 通过 Homebrew 安装即可

```bash
brew install minicom
```

### Setup

1. 将串口线连接到电脑USB上，查看当前命令行查看连接的串口号

```bash
ls /dev/tty*
```

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/serial_port_num.jpg)

> ***上图中的 `/dev/tty.usbserial-14340` 就是 USB 的串口号***

2. 将串口号配置给串口工具 `minicom`

-  命令打开串口工具

```bash
minicom -s
```

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/minicom_board.jpg)

- 通过键盘上的 `J`, `K` 或者方向键上下移动到 `Serial port setup` 选项，按回车选中，进入串口设置界面

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/minicom_setup.jpg)

- 通过键盘上的大写字母 `A`  选中进入 `Serial Device` 选项，并将之前复制好的串口号粘贴在此，并按***两次***回车, 回到主 Setup 界面。
- 定位到 `Save setup as dfl ` 回车并选中。
- 定位到 `Exit` 退出 Setup 界面到 串口命令终端中 或 定位到 `Exit from minicom` 退回到系统命令界面