### Basic Operations of Git

#### Introduction

---

#### Git 常用命令

配置用户信息

文件内（对相关项目文件配置）

> git config user.name “xxx”

> git config user.email “xxx@example.com”

如果不配置用户信息，默认使用全局用户信息

<br/>
配置全局用户信息

> git config —global user.name “xxx”

> git config —global user.email "xxx@example.com"

<br/>
开启颜色显示

> git config —global color.ui auto

<br/>

#### SSH Key

&emsp;&emsp;ssh key 提供了一种与 Git 通信的方式，通过这种方式可以在不输入密码的情况下，将 Git 作为开发者自己的 remote 远端服务器，进行版本控制。

步骤如下：

1. 先检查是否存在 ssh key。<br/>
   终端下&emsp;`ls -al ~/.ssh`

2. 如果存在，请确认是否是自己创建的 ssh key，如果是自己的 ssh key，可以去网页版 Git 上添加 ssh key。没有特殊状况没有必要重新生成新的 ssh key。这只是一对钥匙，只要能互相验证就能够实用。

3. 如果存在，但不是自己的 ssh key，那会对自身操作产生问题。因为可能存在泄密问题，请注意一定要确认狮子生成的 ssh key <br/>
   重新设置 ssh key 的时候，先删除本电脑上原来的远程库&emsp;`git remote remove origin`<br/>
   然后删除掉本电脑上原来 ssh 文件下全部文件&emsp;`rm -r ~/.ssh`， 因为删除原有的 ssh key 之后，再也不可能连上原先的远程仓库了<br/>

4. 为自身重新创建 ssh key <br/>
   终端下&emsp;`ssh-keygen -t rsa -C "xxx@example.com"`<br/>
   按照操作提示完成 ssh key 的创建，<br/>
   默认会在相应的路径下生成 id_rsa(私钥)和 id_rsa.pub(公钥)。

<br/>

#### Git 本地仓库操作

查看 Git 相关结果和帮助<br/>
终端下&emsp;`git`

<br/>
在相关路径下创建工作项目<br/>
> cd 路径

> git init

<br/>

配置个人信息，不配置则使用全局信息

<br/>

查看文件状态<br/>

> git status

<br/>

将工作区文件添加到暂存区<br/>

> git add 所有文件或者指定文件

<br/>

将暂存区文件提交到仓库区<br/>

> git commit -m ‘版本描述 ‘&emsp;&emsp;&emsp;&emsp;会生成一条版本记录<br/>

可合并命令为

> git commit -am&emsp;&emsp;&emsp;&emsp;‘版本描述’

<br/>

查看历史版本<br/>

> git log <br/>

或者<br/>

> git reflog

<br/>

回退版本<br/>

> git reset —hard HEAD<br/>

或者基于版本号回退<br/>

> git reset —hard 版本号

<br/>

撤销修改<br/>
撤销工作区代码 `git checkout 文件`<br/>
撤销暂存区代码 `git checkout 文件名`&emsp;或者&emsp;`git reset HEAD 文件名`（将暂存区代码撤到工作区）

<br/>

对比版本<br/>
`git diff HEAD(版本库) 版本库 文件名`&emsp;对比版本库之间的文件

<br/>

---

#### 克隆项目

克隆远程项目，配置身份信息，创建项目，推送项目到远程仓库<br/>

1. 远程仓库的克隆<br/>
   > cd 项目文件夹下

> git clone 仓库地址

2. 克隆远程仓库到本地<br/>

3. 克隆成功后产看文件<br/>

4. 编辑文件<br/>

5. 推送项目到远程文件<br/>

> git add .

> git commit -m “xxx”

> git push

---

#### github/gitlab 同时管理多个 ssh key

以前用 github 的 ssh key，后来工作原因多了一个 gitlab 的账号，在绑定 gitlab 的 ssh key 时，发现将 github 的 ssh key 覆盖了。怎么同时绑定 github 和 gitlab 的 ssh key，并不产生冲突呢？
今天找到了个小技巧，在.ssh 目录下新建一个 config 文件配置一下，就能解决 gitlab 与 github 的 ssh key 的冲突。

生成并添加第一个 ssh key

```
cd ~/.ssh
ssh  ssh-keygen -t rsa -C "youremail@yourcompany.com"
```

这时可以一路回车，不输入任何字符，将自动生成 id_rsa 和 id_rsa.pub 文件。

生成并添加第二个 ssh key

> \$ ssh-keygen -t rsa -C "youremail@gmail.com"
> 注意，这时不能一路回车，否则邮箱将覆盖上一次生成的 ssh key，给这个文件起一个名字， 比如叫 id_rsa_github, 所以相应的也会生成一个 id_rsa_github.pub 文件。

此时查看 .ssh 下的目录文件，发现多了 id_rsa_github 和 id_rsa_github.pub 两个文件。

添加私钥

```
$ ssh-add ~/.ssh/id_rsa
$ ssh-add ~/.ssh/id_rsa_github
```

修改配置文件
在 ~/.ssh 目录下新建一个 config 文件

> touch config

并添加以下内容

```
# gitlab
Host gitlab.com
    HostName gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_github
```

给 github/gitlab 上添加 ssh key
查看 ssh key 执行‘cat id_rsa_github.pub’内容，将文本内容拷贝到https://github.com/settings/ssh。(拷贝到对应的平台个人设置中)

**测试**

> \$ ssh -T git@github.com

如果输出

```
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

说明成功的连上 github 了。

### git 更新 fork的远程仓库

1. 添加远程仓库到本地remote分支

> git remote add upstream git@github.com:apache/flink.git # 远程仓库地址

2. 查看当前仓库的远程分支

> git remote -v<br/>
> origin  git@github.com:xxx/sample02.git (fetch)<br/>
> origin  git@github.com:xxx/sample02.git (push)<br/>
> upstream  git@github.com:yyy/sample01.git (fetch) # 远程分支<br/>
> upstream  git@github.com:yyy/sample01.git (push)

3. fetch 远程分支

> git fetch upstream

4. 合并 fetch 的分支到 本地master

> git merge upstream/master

5. 查看log最近更新日志 （随意）

> git log

6. 推送 本地master 到 自己的远程仓库（fork代码的自己仓库）

> git push origin master

### 如何将 github 项目上传至 gitlab

#### 一、修改远程分支关联

##### 删除远程分支关联

将原先指向 github 的远程分支关联关系删除

> git remote rm origin

添加新的远程分支关联

新的 remote 地址指向 gitlab 相应地址

> git remote add origin <项目 gitlab 上的 SSH 地址>

修改后可以使用以下命令查看修改是否生效

##### 查看远程分支关联

> git remote -v

#### 二、修改提交用户名

如果 github 与 gitlab 所用用户名和邮箱不一样，可以这么做

修改 gitlab 所用用户名

> git config user.name <gitlab 用户名>
> git config user.email <gitlab 用户邮箱>

修改项目过往提交记录的用户名
如果希望 git 的 log 中的用户名也发生替换，可以这么做

在项目根目录下创建 email.sh 写入下面这段代码(文件的名称随意，不一定要用 email)

```
#!/bin/sh

git filter-branch --env-filter '
OLD_EMAIL="<github 用户邮箱>"
CORRECT_NAME="<gitlab 用户名>"
CORRECT_EMAIL="<gitlab 用户邮箱>"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

把 OLD_EMAIL 、CORRECT_NAME 、 CORRECT_EMAIL 改成自己的新旧邮箱用户名即可；<br/>
创建后记得执行以下命令，让脚本可运行。并提交所有未提交内容，或者 stash(隐藏) 掉。

> chmod 755 email.sh

运行脚本（对应目录下回车 Enter）

> ./email.sh

或者

> cd /data/shell

> sh email.sh

如果遇到如下错误 ：

> Cannot create a new backup. A previous backup already exists in refs/original/ Force overwriting the backup with -f

则执行如下命令；

> git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch Rakefile' HEAD

#### 三、push 内容至 gitlab

1. 推荐使用新分支（gitlab 项目不存在同名分支）提交至 gitlab,比如

> git push --set-upstream origin <新分支名称>

2. 或者，如果想要强制提交，且远程存在相应的分支，可以选择

> git push origin --force --all
