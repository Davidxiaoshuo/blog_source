# macos 远程硬盘挂载

### 准备工作

1. 安装Homebrew, 具体安装方式，见[官网](https://brew.sh/index_zh-cn.html)
2. 通过Homebrew 安装 sshfs 的依赖 `fuse`

 ```bash
brew install Caskroom/cask/osxfuse
```

3. 通过Homebrew 安装sshfs

```bash 
brew install sshfs
```


### 使用

```bash
sshfs remote_account@ip:remote_directory /local/directory
```


> **挂在到本地时，避免挂在到根目录下或者当前账户的主目录下，否则会引起以下错误：**

```
mount_osxfuse: mount point /Users/xxxx/ImageFolder is itself on a OSXFUSE volume
fuse: failed to mount file system: Invalid argument

```
