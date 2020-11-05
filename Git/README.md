## Basic Operations of Git

#### Introduction

---

### Git 常用命令

* 查看 Git 相关结果和帮助<br/>
**终端下**
> git

---

#### 📓准备阶段 #### 

##### 配置用户信息 #####

* 单独配置（定位到相关项目的目录下）
> git config user.name “xxx”<br/>
> git config user.email “xxx@example.com”

**注意**：如果相关项目下不配置用户信息，默认使用*全局用户信息*

* 配置全局用户信息

> git config —global user.name “xxx”<br/>
> git config —global user.email "xxx@example.com"

* 开启颜色显示

> git config —global color.ui auto

<br/>

**注意**：**windows10**环境下可能会报一个错：

```
warning: user.name has multiple values
error: cannot overwrite multiple values with a single value
      Use a regexp, --add or --replace-all to change user.name.
```

这时候可以用另一种修改办法：

> git config --global --replace-all user.name "你的用户名"<br/>
> git config --global --replace-all user.email "你的邮箱" 

---

##### SSH key --- 秘钥的生成和设置

```
    SSH key 提供了一种与 Git 通信的方式，通过这种方式可以在不输入密码的情况下，将 Git 作为开发者自己的远程服务器，进行版本控制。
```

设置步骤如下：

1. 先检查是否存在 **ssh key**。<br/>
**终端下**&emsp;`ls -al ~/.ssh`

2. 如果存在，请确认是否是**自己**创建的 **ssh key**，如果是自己的 **ssh key**，可以去网页版 *Git* 上添加 **ssh key**。没有特殊状况没有必要重新生成新的 **ssh key**。这只是一对密钥，目的就是互相验证。

3. 如果存在，但**不是自己**的 **ssh key**，那会对自身操作产生问题。因为可能存在泄密问题，请注意一定要确认是否是自己生成的 **ssh key** <br/>
   重新设置 **ssh key** 的时候
   1. 删除本电脑上原来的远程库&emsp;`git remote remove origin` （因为删除原有的 **ssh key** 之后，再也不可能连上原先的远程仓库了）<br/>
   2. 删除本电脑上原来 *ssh 文件夹* 下**全部文件**&emsp;`rm -r ~/.ssh`， 

4. 为自己重新创建 **ssh key**<br/>
   **终端下**&emsp;`ssh-keygen -t rsa -C "xxx@example.com"`<br/>
   按照终端窗口的操作提示 完成 **ssh key** 的创建，<br/>
   默认会在相应的路径下生成 *id_rsa(私钥)* 和 *id_rsa.pub(公钥)*。

---

### Git 本地仓库操作

* 在相关路径下初始化项目仓库

> cd 项目仓库路径
> git init

**注意**：配置个人信息，不配置则使用全局信息

* 将工作区文件添加到暂存区

> git add 所有文件或者指定文件

* 查看仓库当前的状态，显示有变更的文件。

> git status

* 将暂存区文件提交到**本地**仓库区

> git commit -m ‘版本描述 ‘

**注意**：上述提交操作指令，可以合并为

> git commit -am ‘版本描述’

*执行完会在本地仓库中生成一条提交记录*

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
> cd 项目文件夹下<br/>
> git clone 仓库地址

2. 克隆远程仓库到本地<br/>

3. 克隆成功后查看本地文件<br/>

4. 编辑文件<br/>

5. 推送项目到远程文件<br/>

> git add .

> git commit -m “xxx”

> git push

---

#### github/gitlab 同时管理多个 ssh key

以前用 **github** 的 **ssh key**，后来工作原因多了一个 *gitlab* 的账号，在绑定 *gitlab* 的 *ssh key* 时，发现将 **github** 的 **ssh key** 覆盖了。怎么同时绑定 **github** 和 *gitlab* 的 ***ssh key***，并不产生冲突呢？
今天找到了个小技巧，在 *.ssh* 目录下新建一个 *config* 文件配置一下，就能解决 *gitlab* 与 **github** 的 ***ssh key*** 的冲突。

生成并添加第一个 **ssh key**

```
cd ~/.ssh
ssh  ssh-keygen -t rsa -C "youremail@yourcompany.com"
```

这时可以一路回车，不输入任何字符，将自动生成 *id_rsa* 和 *id_rsa.pub* 文件。

生成并添加第二个 **ssh key**

> \$ ssh-keygen -t rsa -C "youremail@gmail.com"
> 注意，这时不能一路回车，否则邮箱将覆盖上一次生成的 ssh key，给这个文件起一个名字， 比如叫 id_rsa_github, 所以相应的也会生成一个 id_rsa_github.pub 文件。

此时查看 *.ssh* 下的目录文件，发现多了 *id_rsa_github* 和 *id_rsa_github.pub* 两个文件。

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

**扩展** git更改origin数据源

*三种方式*

* 修改命令
* * > git remote origin set-url URL
* 先删后加
* * > git remote rm origin
* * > git remote add origin git@github.com:xxx/sample03.git
* 直接修改config文件

<br/>

3. fetch 远程分支

> git fetch upstream

4. 合并 fetch 的分支到 本地master

> git merge upstream/master

5. 查看log最近更新日志 （随意）

> git log

6. 推送 本地master 到 自己的远程仓库（fork代码的自己仓库）

> git push origin master

---

### git的不常用指令

#### git修改历史提交commit的描述信息

##### 修改最新的一次commit描述信息

1. 查看当前分支的日志情况（单纯只是确认）

`--oneline` 一条提交信息用一行展示
> git log --oneline

`-n` 查看到此之前的几次提交记录, `n` 可以为 1，2，3，... ，但是实操下来最多展示在此之前 **3** 次提交
> git log -n

> git log -1

2. 执行变更`message`（描述信息）的指令

> git commit --amend

进入 *vim编辑器* 界面， 可修改部分是最上面的一行，`#` 部分为描述和介绍（可以不需要理解，了解最好）<br/>
编辑完后保存，注意使用 vim 命令。

3. 更新远程仓库提交，是远程仓库也修改对应commit的message

> git push

// 如果直接提交，会提示如下信息<br/>
```
! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/XXX/YYYY.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
该信息会提示提交被拒绝，因为git认为本地的分支和线上的不匹配，认为本地分支已过时，需要使用 `git pull` 更新代码。

> git pull origin master

**注意**：pull 则是将远程主机的master分支最新内容拉下来后与当前本地分支直接合并 `fetch + merge`

> git pull origin master

如果远程分支是与当前分支合并，则冒号后面的部分可以省略。如下：

> git pull origin master:feature-wxDemo
> git pull <远程主机名> <远程分支名>:<本地分支名>

// 此时会提示一个 vim编辑器 窗口
```
Merge branch 'master' of https://github.com/XXX/YYYY.git
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```
这个意思是需要将此次合并更新重新提交一个新的commit，并且做好message进行解释，`#`号部分在提交时是忽略的。<br/>
**git**已经默认了一个commit信息（第一行，`#`号上方的），如果想要自己编辑message信息，可以使用**vim**指令进入编辑，删除第一行信息，重新自定义，然后使用 **vim** 指令保存退出。

最后重新提交到远程仓库就可以更改全部的**commit** **message** 信息。

> git push

**特殊操作**：可以无视上面的更新操作，直接强制提交`git push --force`，虽然可以提交成功，但是并不值得推荐在实际应用中使用。

##### 修改更早的commit的message信息

1. 查看自己的提交记录

> git log --oneline<br/>
或者，直接更上一小段commit_ID<br/>
> git log commit_ID<br/>


2. 定位修改的commit位置

> git rebase -i commit_ID<br/>
**注意**：这里的`commit_ID`输入后，会进入到一个**vim**编辑页面，但是只会展示`commit_ID`之后的全部提交的 commit 的 message 信息。所以使用这个方法定位，需要使用期望修改的 commit 的 **前一次** 的 commit。

或者使用，如下代码，**n** 可以是 1，2，3，......，表示在此之前的 **n** 次提交记录<br/>
> git rebase -i HEAD~n

3. 直接在弹出的 **vim** 交互页面上修改，和之前的操作类似。

先将 pick 换成 reword，其余的不懂，保存退出。<br/>
然后进入到对应的commit 的message的编辑页面，在当前进行修改。<br/>
`git pull origin master`，更新代码，然后`git push`，完成对远程仓库的修改。依然**不建议**直接强制提交`git push --force`。


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
