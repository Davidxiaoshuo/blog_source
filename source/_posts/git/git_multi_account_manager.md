---
title: Git 多github（gitlab）账号管理
date: 2019-04-23 16:31:52
tags: git
---

场景：很多时候我们有公司的github的账号同时自己私下还可能会有多个git平台的账号（如：github，gitlab）。这时候在管理git账号，指定哪个项目用哪个账号，放在哪个账户的仓库下，就显得尤为重要了。下面简单说下如何配置git config 文件去管理这些git平台账号。

#### 1. 生成ssh-key

```
$ ssh-keygen -t rsa -C "youremail@company.com” -f ~/.ssh/id-rsa
```

**explain**：这样在~/.ssh/目录下就会生成id-rsa和id-rsa.pub的公钥和私钥。其中id-rsa 我们可以根据我们自己的需求来定义名称，例如我的名称personal-github-id-rsa\(私人），company-github-id-rsa（公司github），company-gitlab-id-rsa（公司gitlab）。生成ssh-key 后我们可以copy 公钥到各git平台

```
$ pbcopy < ~/.ssh/id-rsa
```

通过上面的命令将公钥copy到剪贴板，然后到git平台的ssh-key页面直接粘贴即可。

![](https://raw.githubusercontent.com/Davidxiaoshuo/mybook/master/assets/remote_repository_ssh_url.png)

#### 2. 添加私钥

```
$ ssh-add ~/.ssh/id-rsa
```

将私钥添加到我们本地ssh中

```
# 可以通过 ssh-add -l 来确私钥列表
$ ssh-add -l
# 可以通过 ssh-add -D 来清空私钥列表
$ ssh-add -D
```

#### 3. 创建多账号管理文件

```
touch ~/.ssh/config
vim config
```

```
# gitlab    
Host gitlab.com
     HostName gitlab.com
     PreferredAuthentications publickey
     IdentityFile ~/.ssh/company-gitlab-id-rsa

# github
Host github-company
     HostName github.com
     PreferredAuthentications publickey
     IdentityFile ~/.ssh/company-github-id-rsa

#personal-github
Host github-personal
     HostName github.com
     PreferredAuthentications publickey
     IdentityFile ~/.ssh/personal-github-rsa
```

**explain**: Host 这里可以理解为我们为实际的host（如：github.com）起的别名，HostName才是实际的host address值，这样我们在本地仓库指定远程仓库的时候就可以区分出同一个平台下的不同账号体系了，可以通过下面的remote repository 地址可以看出。

#### 4. 为本地仓库指定远程仓库地址

##### 4.1 远程仓库地址，从github中直接复制显示：

![](https://raw.githubusercontent.com/Davidxiaoshuo/mybook/master/assets/remote_repository_ssh_url.png)

##### 4.2 实际为本地仓库指定远程仓库地址需要这样：

```
添加地址：git remote add origin git@github-personal:Davidxiaoshuo/EmotionCalendar.git

重定向地址：git remote set-url origin git@github-personal:Davidxiaoshuo/EmotionCalendar.git
```

**explain**: 其中git@后面的字段为我们在config配置文件中的Host的值

