## GitNotes

### 前言

****

本文参考廖雪峰的教程、官方文档等网上的资料，整理出来的个人笔记

### 目录

* [Git 环境及基础设置](#Git-环境及基础设置)
* [Git的基础操作](#Git的基础操作)
* [版本控制](#版本控制)
* [撤销工作区的修改、暂存区的暂存](#撤销工作区的修改、暂存区的暂存)
* [远程仓库](#远程仓库)
  * [与github建立连接](#与github建立连接)
  * [本地仓库与远程仓库建立连接](#本地仓库与远程仓库建立连接)
* [分支管理](#分支管理)
  * [分支基础操作](#分支基础操作)
  * [解决分支冲突](#解决分支冲突)
  * [将当前分支的暂存区移动到栈](#将当前分支的暂存区移动到栈)
  * [分支管理之多人协作](#分支管理之多人协作)
  * [分支之变基](#分支之变基)
* [标签](#标签)
* [忽略特殊文件](#忽略特殊文件)

### Git 环境及基础设置

****

* 环境搭建    [参考](../MyWin/WinProgram.md/)
* 用户设定
```bash
git config --global user.name "<username>" # 设置用户名
git config --global user.email <邮箱> # 设置邮箱
git config --global core.editor "'D:/Program/SublimeText3/subl.exe' -w" # 设置默认文本编辑器为sublime文本编辑器
git config --list # 查看配置信息
```

### Git的基础操作

****

*   **本地git仓库的建立**

```bash
git init # 将一个目录变成git仓库
```

*   **工作区文件的状态、暂存、提交**

```bash
git status # 查看仓库文件状态
git diff  # 比较工作区文件与暂存区文件的区别。
git diff --cached # 比较暂存区的文件与当前分支里上次commit后的区别。

git add <file> <file> <file> # 将文件添加到暂存区

git commit # 会打开默认文本编辑器编辑本次提交备注。
git commit -m "<备注>" # 直接编辑备注并提交。
```

### 版本控制

****

每个版本都有一个对应的commit id，同时可以使用下面方法表示：

| 当前版本        | HEAD     |
| --------------- | -------- |
| 上个版本        | HEAD^    |
| 上上个版本      | HEAD^^   |
| ......          | ......   |
| 往上100个版本是 | HEAD~100 |

```bash
git reflog # 查看每个版本的commit id
git log # 查看当前版本过去的提交历史
git reset --hard <commit_id> # 将工作区回到某个版本
```

### 撤销工作区的修改、暂存区的暂存

```bash
git checkout -- <file> # 把文件在工作区的修改、删除都撤销、恢复
git reset HEAD <file> # 把暂存区的文件撤销到工作区
```

### 远程仓库

****

#### 与github建立连接

```bash
ssh-keygen -t rsa -C "<邮箱>" # 创建SSH Key
    # 用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，id_rsa.pub是公钥。
    # 在GitHub上添加公钥。
```

####  本地仓库与远程仓库建立连接

* 一般情况先在github上建立仓库，然后`git clone`到本地

```bash
git clone <url> # 从远程现有仓库克隆
git remote -v # 查看远程库的信息
git push  <远程库名 = origin> <分支 = master> # 推送送到远程仓库
```

* 先在本地建立仓库，然后添加远程库

```bash
git remote add <远程库名 = origin> git@github.com:<用户名>/<仓库名>.git # 建立连接
git push -u <远程库名 = origin> <分支 = mastermaster> # 送到远程第一次推送使用-u参数，关联仓库 
```

*   远程库管理

```bash
git remote -v # 查看远程库的信息
git remote rm <远程库名 = origin> # 删除已有的远程库
```

### 分支管理

****

#### 分支基础操作

```bash
git branch # 查看分支
git branch <branch-name> # 创建分支
git checkout <branch-name> # 切换分支
git checkout -b <branch-name> # 创建并切换分支
git merge <name> # 合并某分支到当前分支，使用Fast forward
git merge --no-ff -m "<备注>" <branch-name> #合并某分支到当前分支，禁用Fast forward：多进行一次commit,保留合并分支消息
git branch -d <branch-name> # 删除分支
git branch -D <branch-name> # 强制删除分支，可以删除未合并的分支(无法恢复)

```
#### 解决分支冲突

合并分支时，如果当前分支有新的提交时，合并就会冲突，需要手动解决冲突在重新commit

```bash
git log --graph # 查看分支合并图
```

#### 将当前分支的暂存区移动到栈

```bash
git stash # 隐藏当前分支的暂存区
git stash list # 查看栈里的暂存区
git stash apply <> # 恢复
git stash drop <> # 删除
git stash pop <> # 恢复并删除
```

#### 分支管理之多人协作

```bash
git push  <远程库名 = origin> <分支 = master> #试图推送自己的修改
git pull # 提示推送失败（是因为远程分支比本地分支的版本新）用此命令尝试合并冲突
	# 如果继续提示失败则使用下面命令继续 git push
git branch --set-upstream-to=origin/<branch> <branch> #创建本地分支和远程分支的链接关系
	# git push 成功后，如果合并有冲突，则手动解决冲突后commit。
git push origin <branch-name> # 解决冲突后继续提交
```

#### 分支之变基

[Git - 分支 - 变基官方文档](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)

```bash
git rebase <branch-name> # 将当前分支变基到目的分支去，使分支合并图看起来是一条直线
```

### 标签

>   标签：`commit id`的`域名`
>   `commit id` 和`标签`的关系就是`ip`和`域名`的关系

*   创建标签

```bash
git tag # 查看所有标签
git tag <tagname> # 给当前分支的最新一次commit打标签
git tag v0.9 <tagname> <commit id> # 给某一次commit打标签
git tag -a <tagname> -m "说明" <commit id> # 带说明的打标签
```

*	标签操作

```bash
git show <tagname> # 查看标签信息
git tag -d <tagname> # 删除标签
git push origin <tagname> # 推送一个标签到远程库（commit时默认不提交）
git push origin --tags # 一次性推送全部标签
```

*   删除远程标签

```bash
git tag -d <tagname> # 先从本地删除
git push origin :refs/tags/<tagname> # 然后从远程删除
```

### 忽略特殊文件

*   [GitHub各种`.gitignore`配置文件仓库](https://github.com/github/gitignore)
*   在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后吧把`.gitignore`也commit到仓库
*   文件 `.gitignore` 的格式规范如下：
    *   所有空行或者以 `＃` 开头的行都会被 Git 忽略。
    *   可以使用标准的 glob 模式匹配。
    *   匹配模式可以以（`/`）开头防止递归。
    *   匹配模式可以以（`/`）结尾指定目录。
    *   要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

    >    glob 模式
    >
    >   |   `*`   |                    匹配零个或多个任意字符                    |
    >   | :-----: | :----------------------------------------------------------: |
    >   | `[abc]` |                匹配任何一个列在方括号中的字符                |
    >   |   `?`   |                       匹配一个任意字符                       |
    >   | `[1-9]` |                  表示匹配所有 1 到 9 的数字                  |
    >   |  `**`   | 匹配任意中间目录比如`a/**/z` 可以匹配 `a/z`, `a/b/z` 或 `a/b/c/z`等。 |

	```bash
	git add -f App.class # 强制添加到Git
	git check-ignore -v <file name> # 查看某文件是否被.gitignore了
	```
