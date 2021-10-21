## Basic Operations of Git

#### Introduction

---

### Git 常用命令

- 查看 Git 相关结果和帮助<br/>
  **终端下**
  > git

---

#### 📓 准备阶段

##### 配置用户信息

- 单独配置（定位到相关项目的目录下）
  > git config user.name “xxx”<br/>
  > git config user.email “xxx@example.com”

**注意**：如果相关项目下不配置用户信息，默认使用*全局用户信息*

<br/>

- 配置全局用户信息

> git config —global user.name “xxx”<br/>
> git config —global user.email "xxx@example.com"

<br/>

- 开启颜色显示

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

2. 如果存在，请确认是否是**自己**创建的 **ssh key**，如果是自己的 **ssh key**，可以去网页版 _Git_ 上添加 **ssh key**。没有特殊状况没有必要重新生成新的 **ssh key**。这只是一对密钥，目的就是互相验证。

3. 如果存在，但**不是自己**的 **ssh key**，那会对自身操作产生问题。因为可能存在泄密问题，请注意一定要确认是否是自己生成的 **ssh key** <br/>
   重新设置 **ssh key** 的时候

   1. 删除本电脑上原来的远程库&emsp;`git remote remove origin` （因为删除原有的 **ssh key** 之后，再也不可能连上原先的远程仓库了）<br/>
   2. 删除本电脑上原来 _ssh 文件夹_ 下**全部文件**&emsp;`rm -r ~/.ssh`，

4. 为自己重新创建 **ssh key**<br/>
   **终端下**&emsp;`ssh-keygen -t rsa -C "xxx@example.com"`<br/>
   按照终端窗口的操作提示 完成 **ssh key** 的创建，<br/>
   默认会在相应的路径下生成 _id_rsa(私钥)_ 和 _id_rsa.pub(公钥)_。

**补充**：免密 push、pull
在配置好`ssh`添加到远程仓库

方法一
执行下面代码（接下来输入密码就 ok 了）

> ssh-add -K ~/.ssh/id_rsa

方法二
删除 passphrase 密码

> ssh-keygen -p

---

### 📝Git 本地仓库操作

##### 创建仓库指令

- 在相关路径下初始化项目仓库

> cd 项目仓库路径
> git init

<br/>

- 拷贝一份远程仓库，也就是下载一个项目。（需要获得远程仓库地址）

> git clone <远程仓库地址>

**注意**：配置个人信息，不配置则使用全局信息

**注意**：如果项目中含有**子模块**（Git submodule），如果需要一次性下载全部的代码。

> git clone --recursive <远程仓库地址>

---

##### 提交与修改指令

- 将工作区文件添加到暂存区

> git add 所有文件或者指定文件

<br/>

- 查看仓库当前的状态，显示有变更的文件。

> git status

<br/>

- 将暂存区文件提交到**本地**仓库区

> git commit -m ‘版本描述 ‘

<br/>

**注意**：上述提交操作指令，可以合并为

> git commit -am ‘版本描述’

_执行完会在本地仓库中生成一条提交记录_

<br/>

- 比较文件的不同，即暂存区和工作区的差异。

> git diff

        1. git diff：当工作区有改动，临时区为空，diff的对比是“工作区与最后一次commit提交的仓库的共同文件”；当工作区有改动，临时区不为空，diff对比的是“工作区与暂存区的共同文件”。
        2. git diff --cached 或 git diff --staged：显示暂存区(已add但未commit文件)和最后一次commit(HEAD)之间的所有不相同文件的增删改( git diff --cached 和 git diff –staged 相同作用)
        3. git diff HEAD：显示工作目录(已track但未add文件)和暂存区(已add但未commit文件)与最后一次commit之间的的所有不相同文件的增删改。
            $ git diff HEAD~X 或 git diff HEAD^^^…(后面有X个^符号，X为正整数):可以查看最近一次提交的版本与当前版本回退X个版本的版本之间的所有工作目录(已track但未add文件)和暂存区(已add但未commit文件之间的增删改。
        4. git diff <分支名1> <分支名2> ：比较两个分支上最后 commit 的内容的差别
            $ git diff branch1 branch2 --stat        显示出所有有差异的文件(不详细,没有对比内容)
            $ git diff branch1 branch2               显示出所有有差异的文件的详细差异(更详细)
            $ git diff branch1 branch2 具体文件路径    显示指定文件的详细差异(对比内容)

<br/>

- 回退版本，可以指定退回某一次提交的版本。

> git reset

```
git reset [--soft | --mixed | --hard] [HEAD]
```

        --mixed 为默认，可以不用带该参数，用于重置暂存区的文件与最近一次的提交(commit)保持一致，工作区文件内容保持不变。
            $ git reset HEAD^             # 回退所有内容到上一个版本
            $ git reset HEAD^ hello.js    # 回退 hello.js 文件的版本到上一个版本
            $ git reset 052exxxx          # 回退到指定版本

        --soft 用于回退到某个版本。
            $ git reset --soft HEAD~3     # 回退上上上一个版本

        --hard 撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交。
            $ git reset –-hard HEAD~3           # 回退上上上一个版本
            $ git reset –-hard bae128           # 回退到某个版本回退点之前的所有信息。
            $ git reset --hard origin/master    # 将本地的状态回退到和远程的一样

<br/>

- 删除工作区文件

> git rm

        git rm <file>     命令本质上就是先执行了 rm 文件名，然后执行 git add 把 rm命令 提交到暂存了。
        git rm -f <file>  如果该文件已经修改过并且已经放到暂存区域（git add）的话，则必须要用强制删除选项 -f。
        git rm --cached <file>  如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录中（仅是从跟踪清单中删除）。

<br/>

- 移动或重命名工作区文件

> git mv

        git mv [file] [newfile]  如果新但文件名已经存在，但还是要重命名它，可以使用 -f 参数
        git mv [file] [route/newfile]  移动文件并且重新命名（该文件在移动前不能修改）

<br/>

- 查看历史版本

> git log

<br/>

- 查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

> git reflog

<br/>

- 是以列表形式显示修改记录

> git blame <file>

---

- `git checkout` 实用指令详解

        1. git checkout                    表示核查工作区相对于最近一次版本修改过的全部文件
        2. git checkout --help             获取 git checkout 的 使用文档
        3. git checkout 分支名              表示切换分支
        4. git checkout -b 分支名           表示以当前分支的当前状态创建新分支并切换到新分支
            $ git checkout -b 分支名 <commitID>      表示以当前分支的 commitID 提交节点创建新的分支并切换到新分支。工作区的内容保留到新分支。
        5. git checkout <commitID>         以指定的提交节点创建了一个 临时性分支，此临时性分支可用于做实验性修改
        6. git checkout <filename>         当没有提交版本号时将工作区的指定文件的内容恢复到暂存区的状态
            $ git checkout .               将工作区的所有文件的内容恢复到暂存区的状态
        7. git checkout <commitID> <filename>       当有提交版本号时，表示将工作区和暂存区都恢复到版本库指定提交版本的指定文件的状态,此时 HEAD 指针不变，此时的状态相当于把工作区的内容修改到指定版本的文件内容后，再把修改的内容添加到暂存区。(git checkout <commit> <filename>后，可以直接执行git commit而不需要先执行git add)

---

#### 🚀Git 远程操作

- 远程仓库操作

  > git remote

          1. git remote -v                        显示所有远程仓库（别名和地址）
          2. git remote show [url]                显示某个远程仓库的信息，需要远程仓库地址
          3. git remote add [shortname] [url]     添加远程版本库，shortname 为版本库别名，url 为版本库远程地址
              $ git remote rm [shortname]         删除本地设置的远程仓库
              $ git remote rename [shortname] [new_name]      修改远程仓库的别名

<br/>

- 从远程获取代码库

  > git fetch

          $ git fetch [alias] 获取远程仓库 alias 的更新数据
          $ git merge [alias]/[branch]    将远程仓库(alias)的任何更新合并到当前分支(branch)

<br/>

- 从远程获取代码并合并本地的版本(git fetch 和 git merge 的结合指令)

  > git pull

          $ git pull <远程主机名> <远程分支名>:<本地分支名>
          $ git pull <远程主机名> <远程分支名>        当远程分支是与当前分支合并，则可以省略 本地分支名

<br/>

- 将本地的分支版本上传到远程仓库并合并

  > git push

          $ git push <远程主机名> <本地分支名>:<远程分支名>
          $ git push <远程主机名> <本地分支名>        当本地分支名与远程分支名相同，则可以省略 远程分支名

---

### Git 远程协作开发

#### fork 远程仓库

1. 在 **git** 网站上，对他人的项目点击 `fork`，在自己的仓库中得到一封自己本地的 copy 版本

2. 添加远程仓库到本地 remote 分支

> git remote add upstream git@github.com:apache/flink.git 远程仓库地址

3. 可以确认当前仓库的远程分支

> git remote -v

```
$ origin  git@github.com:xxx/sample02.git (fetch)
$ origin  git@github.com:xxx/sample02.git (push)
$ upstream  git@github.com:yyy/sample01.git (fetch)        远程分支
$ upstream  git@github.com:yyy/sample01.git (push)
```

4. `fetch` 远程仓库，获取全部分支的更新内容

> git fetch upstream 可以在远程仓库后加上对应的分支名

5. 合并 `fetch` 的远程分支到 **本地 master**

> git merge upstream/master

6. 可以选择性地查看*log*最近提交日志

> git log

7. 项目本地编写操作，提交修改至**本地仓库区**

8. 推送 **本地 master** 到 自己的远程仓库（fork 代码的自己仓库）对应的分支

> git push origin master

**注意：**如果远程仓库没有当前分支

> git push --set-upstream origin <当前分支名称>

---

### Git 指令扩展

#### git 修改历史提交 commit 的描述信息

##### 修改最新的一次 commit 描述信息

1. 查看当前分支的日志情况（单纯只是确认）

`--oneline` 一条提交信息用一行展示

> git log --oneline

`-n` 查看到此之前的几次提交记录, `n` 可以为 1，2，3，... ，但是实操下来最多展示在此之前 **3** 次提交

> git log -n

> git log -1

2. 执行变更`message`（描述信息）的指令

> git commit --amend

进入 **_vim 编辑器_** 界面， 可修改部分是最上面的一行，`#` 部分为描述和介绍（可以不需要理解，了解最好）<br/>
编辑完后保存，注意使用 vim 命令。

3. 更新远程仓库提交，是远程仓库也修改对应 **commit** 的 **message**

> git push

如果直接提交，会提示如下信息

```
! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/XXX/YYYY.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

该信息会提示提交被拒绝，因为 **git** 认为本地的分支和线上的不匹配，认为本地分支已过时，需要使用 `git pull` 更新代码。

> git pull origin master

**注意**：pull 则是将远程主机的 master 分支最新内容拉下来后与当前本地分支直接合并 `fetch + merge`

> git pull origin master

如果远程分支是与当前本地分支名称一致，则冒号后面的部分可以省略。如下：

> git pull origin master:feature-wxDemo
> git pull <远程主机名> <远程分支名>:<本地分支名>

此时会提示一个 vim 编辑器 窗口

```
Merge branch 'master' of https://github.com/XXX/YYYY.git
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

这个意思是需要将此次合并更新重新提交一个 **_新的_** **commit**，并且编辑好 **message** 进行解释，`#`号部分在提交时是忽略的。<br/>
**git**已经默认了一个 **commit** 信息（第一行，`#`号上方的），如果想要编辑 **message** 信息，可以使用**vim**指令进入编辑，删除第一行信息，重新编写，然后使用 **vim** 指令保存退出。

最后重新提交到远程仓库就可以更改全部的**commit** **message** 信息。

> git push

**特殊操作**：可以无视上面的更新操作，直接强制提交`git push --force`，虽然可以提交成功，但是并不值得推荐在实际应用中使用。

---

##### 修改更早的 commit 的 message 信息

1. 查看自己的提交记录

> git log --oneline

或者在指令后加上一小段 _commit_ID_

> git log commit_ID

2. 定位修改的 **commit** 位置

> git rebase -i commit_ID

**注意**：这里的`commit_ID`输入后，会进入到一个**vim**编辑页面，但是只会展示`commit_ID`之后的全部提交的 **commit** 的 **message** 信息。所以使用这个方法定位，需要使用期望修改的 **commit** 的 **前一次** 的 **commit**。

或者使用，如下代码，**n** 可以是 1，2，3，......，表示在此之前的 **n** 次提交记录

> git rebase -i HEAD~n

3. 直接在弹出的 **vim** 交互页面上修改，和之前的操作类似。

先将 `pick` 换成 `reword`，保存退出。<br/>
然后进入到对应的 **commit** 的 **message** 的编辑页面，在当前操作状态栏下进行修改。<br/>
`git pull origin master`，更新代码，然后`git push`，完成对远程仓库的修改。依然**不建议**直接强制提交`git push --force`。

####

---

### Git 扩展操作

#### github/gitlab 同时管理多个 ssh key

&emsp;&emsp;在应对个人学习开发和工作中的任务开发，可能会同时使用 **github** 和 **_gitlab_**。在为了方便代码管理，会分别设置 **_ssh key_**，但是如果按照传统操作，可能会造成 新生成设置的 **_ssh key_** 覆盖了 原先的 **ssh key**。为了能够同时绑定 **github** 和 **_gitlab_**，不并且不造成冲突，需要在 _.ssh_ 目录下新建一个 _config_ 文件并且做出相关配置。

1. 在 _.ssh_ 目录下新建一个 `config` 文件

```
$ cd ~/.ssh
$ touch config
```

2. 生成并添加第一个 **ssh key**

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -C "youremail@yourcompany.com"
```

自动生成 _id_rsa_ 和 _id_rsa.pub_ 文件（可以回车确认到结束，不输入任何字符，）。

3. 生成并添加第二个 **ssh key**

```
$ ssh-keygen -t rsa -C "youremail@gmail.com"
```

**注意**：这一步操作不能一路回车确认到最后，否则邮箱将覆盖上一次生成的 **ssh key**，将即将生成的*秘钥文件*重新命名， 例如命名为 `id_rsa_github`，相应的也会生成一个 `id_rsa_github.pub` 文件。

此时查看 _.ssh_ 下的目录文件，发现多了 `id_rsa_github` 和 `id_rsa_github.pub` 两个文件。

4. 添加私钥

```
$ ssh-add ~/.ssh/id_rsa
$ ssh-add ~/.ssh/id_rsa_github
```

5. 修改配置文件

在 _~/.ssh_ 目录下 `config` 文件中添加以下内容

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

为 **github**/**_gitlab_** 上添加 **ssh key**<br/>
执行 `cat id_rsa_github.pub` / `cat id_rsa.pub`，查看 对应平台的 **ssh key** 内容，将文本内容拷贝到对应的平台个人设置中。
为了避免每次 push 都要输入账号密码。**[必要]**使用`ssh地址`而非`https地址`的方式 `git clone`，或者改变 remote 远程 url 为 ssh。

6. **测试** 设置是否成功

以 **github** 为例

> \$ ssh -T git@github.com

如果命令栏输出

```
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

说明成功的连接上 github 了，设置成功。

---

#### 如何将 github 项目上传至 gitlab

##### 一、修改远程分支关联

1. 删除原先的远程分支关联，将原先指向 **github** 的远程分支关联关系删除

   > git remote rm origin

2. 添加新的远程分支关联，新的 `remote` 地址指向 **_gitlab_** 相应地址
   > git remote add origin <项目 gitlab 上的 SSH 地址>

修改后可以查看远程分支关联修改是否生效

> git remote -v

##### 二、修改提交用户名

- 如果 **github** 与 **_gitlab_** 所用*用户名*和\*邮箱**\*不一样**，需要修改

> git config user.name <gitlab 用户名><br/>
> git config user.email <gitlab 用户邮箱>

- **修改项目过往提交记录的用户名**

**注意**：如果希望 **git** 的 `log` 中的*用户名*也发生替换，需要进行对应的配置

在项目根目录下创建 `email.sh` 输入下面这段代码(文件的名称随意，不一定要用 email)

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

**注意**：将 **_OLD_EMAIL_** 、**_CORRECT_NAME_** 、 **_CORRECT_EMAIL_** 改成自己的新旧邮箱用户名即可；<br/>
创建后需要执行以下命令，保存编辑结束后的内容，并且赋予让脚本文件 _读写执行_ 权限。

> chmod 755 email.sh

运行脚本（对应目录下回车 Enter）

> ./email.sh

或者

> cd /data/shell<br/>
> sh email.sh

如果遇到如下错误 ：

```
Cannot create a new backup. A previous backup already exists in refs/original/ Force overwriting the backup with -f
```

执行如下命令

> git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch Rakefile' HEAD

##### 三、将对应仓库内容 push 至 gitlab

- 推荐使用新分支（**_gitlab_** 项目当前不存在其他分支）提交至 **_gitlab_**

> git push --set-upstream origin <新分支名称>

- 如果想要强制提交，且远程存在相应的分支，可以选择

> git push origin --force --all

---

### Submodule 管理项目子模块

#### 常用命令

> git clone <repository> --recursive 递归的方式克隆整个项目<br/>
> git submodule add <repository> <path> 添加子模块<br/>
> git submodule init 初始化子模块<br/>
> git submodule update 更新子模块<br/>
> git submodule foreach git pull 拉取所有子模块<br/>
