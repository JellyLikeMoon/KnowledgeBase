# git ssh 登录

- 设置 ssh key

参数说明:`-t` 选择算法; `-b` 设置长度; `-C` 添加说明信息; `-f` 指定密钥对输出位置  
可为私钥设置密码,不设置直接回车

```powershell
PS C:\Users\XX\OneDrive\Documents\Markdown> ssh-keygen.exe -t rsa -b 4096 -C xxx -f "C:\Users\XX\Downloads\ssh-key-test"
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\XX\Downloads\ssh-key-test
Your public key has been saved in C:\Users\XX\Downloads\ssh-key-test.pub
The key fingerprint is:
SHA256:gql/lJ05i3rkOMM/lyMaUZW5K00XMO8aZdsUuFkm2zs xxx
The key's randomart image is:
+---[RSA 4096]----+
|        ++ ..    |
|       .oo+ o.   |
|      .  .+X.    |
|     +  o+=+.    |
|    + .=S=o ..   |
|   . .=.Bo  E    |
|  ...= o.+   .   |
|   .=.B =        |
|    +O.+ .       |
+----[SHA256]-----+

```

- 本地配置文件

`C:\Users\XX\.ssh\config` 文件中添加如下内容:

```bash
Host github.com
    HostName github.com
    User git
    IdentitiesOnly yes
    IdentityFile "C:\Users\XX\.ssh\id_rsa_github"

```

- github 设置

登录 GitHub, 点击右上角头像, 依次点击 Settings -> SSH and GPG keys -> New SSH Key, 添加`id_rsa.pub`的内容到选择框, 点击确认即可.

---

# Git 用法

- 初始化仓库

初始化一个新的 git 仓库, 在空目录或者现有项目的根目录运行, 会创建一个`.git`目录, 该目录包含所有版本控制信息

```bash
# 初始化当前目录为仓库
git init

# 指定初始化仓库目录
git init <目录路径>

# 使用"--bare"选项初始化仓库, 用于创建没有工作区的裸仓库(不包含项目文件, 且不可逆为普通仓库), 通常用于远程仓库或共享仓库环境中
git init --bare

```

- 克隆

克隆一个远程仓库到本地, 克隆仓库时会包含所有历史记录, 可以使用"`--depth 1`"进行浅克隆, 仅克隆最新的提交, 以节省空间和时间

```bash
# 克隆公共仓库到当前目录
git clone https://github.com/user/repo.git

# 克隆到指定目录
git clone https://github.com/user/repo.git my-local-repo

# 使用ssh克隆仓库
git clone git@github.com:user/repo.git

# 克隆指定分支
git clone -b branch-name https://github.com/user/repo.git

```

- 配置

  - 设置 Git 的配置信息, 使用"`git config --global`"为全局设置, 使用"`git config --local`"为当前仓库设置, 使用"`git config --list`"检查配置信息  
  - `--global`配置全局信息, 应用于所有仓库, 存储在`.gitconfig`  
  - `--local`应用于当前仓库,存储在`.git/config`  
  - 配置生效优先级: 本地>全局>系统

```bash
# 仅针对当前仓库生效
git config --local user.name "xx"
git config --local user.email "xx@xx.com"

# 针对所有仓库生效
git config --global user.name "xx"
git config --global user.email "xx@xx.com"

# 查看配置信息
git config --list

# 设置默认编辑器
git config --global core.editor "code --wait"

# 设置换行符处理方式
git config --global core.autocrlf true          # 针对Windows, 自动将LF转为CRLF
git config --global core.autocrlf input         # 针对Linux, MAC, 保持LF不变

# 设置合并工具和差异工具
git config --global merge.tool meld
git config --global diff.tool meld

# 设置git别名(git status设置为git st)
git config --global alias.st status

# 设置颜色显示
git config --global color.ui auto

# 配置默认推送行为
git config --global push.default simple
```

- 暂存

将工作区的修改添加到暂存区, 使用"`git add .`"会添加所有改动, 可使用"`git add <文件名>`"逐个添加文件

```bash
# 添加单文件到暂存区
git add <文件名>

# 添加多文件到暂存区
git add <文件1> <文件2> ...

# 添加整个目录中的文件到暂存区
git add <目录名>/

# 添加所有修改文件到暂存区
git add .

# 添加指定类型文件到暂存区, 可使用通配符匹配名称, 当使用通配符添加文件时, Git只会匹配当前目录下的文件. 如果需要递归添加文件，需要配合"**"语法
git add *.html
git add **/*.html

# 交互式添加文件, 使用"-p"选项可以分段添加文件的更改, 允许你逐行或逐块选择哪些更改需要暂存
git add -p

# 更新已追踪文件, 只暂存已被Git追踪的文件(忽略新增文件)
git add -u

# 添加所有文件更新到暂存区, 使用"-A"可以确保所有文件, 包括新增, 修改和删除的文件都被暂存
git add -A

```

- 提交

  - `git commit` 命令用于将暂存区中的更改提交到本地 Git 仓库.  
  - 提交操作会将暂存区的文件变化打包成一次提交(commit), 并记录提交信息(message), 生成唯一的提交标识符(SHA-1 哈希值).  
  - `git commit` 是 Git 版本控制的核心操作, 它记录了项目的变更历史, 并允许追踪每次修改的内容, 时间及原因.

```bash
# 为提交信息填写说明
git commit -m "xxx"

# 交互式编辑模式提交信息
git commit

# "-a"选项会自动将所有已追踪文件的更改添加到暂存区并提交, 不包括新添加及被删除的文件, 该选项不会处理这些文件, 需要提前使用"git add"或"git rm"手动加入暂存区
git commit -a -m "提交说明"

# 修改最后一次提交信息, 只应该本地使用, 已提交至远程仓库后使用该选项会导致历史记录冲突, 因为会修改提交的哈希值, 等于重写提交历史
git commit --amend

# 可以选择性的提交文件的部分改动, 允许分段提交代码
git commit -p

# 带签名的提交
git commit -S -m "提交说明"

# 空提交, 不包含任何更改的提交
git commit --allow-empty -m "Empty commit for version bump"

```

- 忽略提交文件

  - 指定文件或目录不进行版本控制, 不允许提交到仓库, 需要忽略, 该文件"`.gitignore`"存放在仓库根目录  
  - "`.gitignore`"仅针对当前目录及子目录, 可在不同目录中创建不同的"`.gitignore`"文件  
  - "`.gitignore`"仅针对尚未被追踪的文件生效, 如已被追踪(提交), 即使添加到"`.gitignore`"也会被继续提交, 可使用"`git rm --cached <文件名>`"先从 git 记录中删除

```bash
# 忽略单文件,直接在".gitignore"中填写文件名
debug.log

# 忽略目录, 目录名后须添加"/"
src/

# 使用通配符忽略指定类型文件,
*.log

# 忽略子目录中的文件
src/logs/*.log

# 忽略某目录的所有内容, 但保留某文件或目录, 具体的文件路径优先级高于通配符规则
temp/*
!temp/keep.txt

# 全局忽略文件, 设置全局".gitignore"文件信息
git config --global core.excludesfile ~/.gitignore_global

# 检查文件是否被忽略
git check-ignore -v <文件路径>

# 忽略已被提交文件, 先从git记录中删除
git rm --cached <文件名>

```

- 当前状态

查看当前工作区和暂存区的文件状态, 已修改文件, 已暂存文件, 未被追踪文件, 准备提交的文件等信息

符号含义 :  
`M` : 修改(modified), 文件已被修改  
`MM` : 已修改并已暂存  
`T` : 文件类型改变(file type changed)
`A` : 已添加(Added), 文件已暂存  
`D` : 删除(deleted), 文件已被删除  
`R` : 重命名(renamed), 文件已被重命名  
`??` : 未被追踪(untracked), 新文件尚未被添加到版本控制  
`UU` : 合并冲突(merge conflict), 文件存在为解决的合并冲突

```bash
# 简洁输出暂存区信息
git status -s

# 显示分支信息
git status --branch

```

- 提交历史

查看git仓库的提交历史, 包括提交的哈希值, 提交者, 提交时间, 提交信息, 可以了解项目的历史演进, 查看每次提交的详细信息, 可以根据需要筛选, 格式化和搜索提交记录

```bash
# 默认会按照时间倒序显示所有提交记录
git log

# 简洁输出
git log --oneline

# 限制输出的提交数量
git log -n 5

# 查看指定时间段内的提交
git log --since="2023-01-01" --until="2023-12-31"

# 查看指定作者的提交
git log --author="John Doe"

# 查看提交信息中符合查找关键字的提交
git log --grep="bug fix"

# 查看提交差异, 会输出所有提交的详细变更
git log -p

# 查看指定文件的提交历史
git log -- <filename>

# 自定义日志输出格式
git log --pretty=short

# 图形化输出提交历史
git log --graph --oneline --all

# 默认会显示合并提交, 显示非合并提交
git log --no-merges

# 统计每个提交的文件更改, 显示每次提交更改哪些文件及更改的行数统计
git log --stat

```

- 引用历史

  - 记录所有对引用进行更新的日志, 即使提交被`git reset`或`git checkout`等操作删除, 仍然会保留这些操作的历史.
  - 与`git log`不同, `git reflog`记录的是本地仓库所有对引用的移动或修改, 包括`checkout`, `reset`, `rebase`, `merge`等操作.
  - `reflog`是一种本地操作, 只存在于当前本地仓库, 不会被推送到远程仓库中.
  - `reflog`默认仅保存90天


```bash
# 查看HEAD的引用历史
git reflog

# 查看指定引用的日志, 除HEAD, 还可以查看其它分支或标签的reflog
git reflog show main

# 删除引用日志, "--expire"选项为指定日期
git reflog expire --expire=90.days

```

- 删除

`git rm`用于从git仓库中删除文件或目录, 同时从工作区中删除这些文件, 并且将删除操作记录到暂存区中.  
删除文件将从工作区中被删除, 如已被提交, 仅能通过历史提交恢复.  
git rm只能删除已被git追踪的文件, 未被追踪的文件(处于untracked file的部分), 则需要手动删除.

```bash
# 删除文件并将删除操作记录到暂存区
git rm <file>

# 删除git仓库文件但保留工作区文件
git rm --cached <file>

# 递归删除目录及其内容
git rm -r <directory>/

# 强制删除
git rm -f <file>

# 可使用通配符匹配名称
git rm *.log

```

- 比较

  - 比较git仓库中不同版本, 分支, 提交之间的差异, 可以详细了解文件得到修改内容, 包括新增, 删除, 更改的代码. 用于查看工作目录中的未提交改动, 暂存区中的变更以及不同提交之间的差异  
  - 默认仅比较工作区和暂存区的差异

```bash
# 查看工作目录中的未暂存更改, 比较工作区和暂存区的差异, 显示未暂存更改
git diff

# 查看暂存区与上次提交的差异, 比较暂存区与上次提交的差异, 显示哪些更改已暂存当未提交
git diff --cached

# 查看工作目录与上次提交的整体差异, 比较工作区与上次提交的差异, 包括已暂存和未暂存的更改
git diff HEAD

# 比较分支之间的差异
git diff <branch1> <branch2>

# 比较两次提交之间的差异
git diff <commit1> <commit2>

# 比较暂存区和特定提交之间的差异
git diff --cached <commit>

# 查看指定文件的差异
git diff <file>

# 比较目录之间的差异
git diff <directory>/

# 查看统计摘要, 如只查看变更的统计信息(新增或删除的行数), 使用"--stat"
git diff --stat

# 显示简洁差异摘要, "--name-only"只显示文件名, 不显示具体差异
git diff --name-only

# 忽略空格差异或行尾空格
git diff -w
git diff --ignore-space-at-eol

# 比较特定提交的差异, 查看某次提交和它的父提交之间的差异
git diff <commit>

# 比价两个标签之间的差异
git diff <tag1> <tag2>

```

- 撤销提交

git回退到某个指定的提交或状态, 可以撤销提交, 取消暂存区文件或修改当前分支的指针位置  

```bash
# 回退到某个提交并保留更改(默认行为), 将HEAD回退到指定提交, 但保留工作目录中的更改
git reset <commit>

# 回退到某个提交并保留工作区文件(soft reset), 只回退提交历史, 暂存区和工作目录保持不变, 适合用于撤销提交但保留修改
git reset --soft <commit>

# 回退到某个提交并取消暂存文件(mixed reset), 回退提交历史和暂存起来,但保留工作目录中未提交文件
git reset --mixed <commit>

# 回退到某个提交并丢弃所有更改(hard reset), 回退提交历史, 暂存区和工作目录, 所有未提交的更改会被丢弃
git reset --hard <commit>

# 仅取消文件的暂存状态, 仅将某些文件从暂存区移回工作目录, 不影响提交历史
git reset <file>

# 回退到一个远程分支的状态, 将当前分支重置为远程分支的状态, 撤销本地的更改
git reset --hard origin/main

# HEAD表示当前分支的最新提交, 通过HEAD~n恢复到指定引用记录
git reset --hard HEAD@{3}

# 恢复到指定的提交
git reset --hard 98b4cd9

```

- 远程仓库

管理git本地仓库和远程仓库的地址, 可用于查看, 添加, 删除, 修改远程仓库的URL信息, 可以和多个远程仓库进行交互, 推送本地更改或者拉取远程更新, 远程仓库地址为`HTTPS`或`SSH`格式, `SSH`可以避免频繁的认证.  

```bash
# 查看当前远程仓库
git remote

# 查看远程仓库的详细信息, 包括拉取(fetch)和推送(push)地址
git remote -v

# 添加远程仓库, "<name>"为远程仓库的别名
git remote add <name> <url>

# 移除远程仓库
git remote remove <name>

# 修改远程仓库地址
git remote set-url <name> <new-url>

# 重命名远程仓库
git remote rename <old-name> <new-name>

# 获取远程仓库信息
git remote show <name>

# 删除无效远程分支, 当远程分支也被删除, 本地仍保留该分支时, 可以清理
git remote prune <name>

# 分别设置推送和拉取仓库地址
git remote set-url origin --push git@github.com:username/repository.git
git remote set-url origin --fetch https://github.com/username/repository.git
```

- 标签

  - 为git仓库中的某个特定的提交(`commit`)创建一个标签, 为某个版本做标记, 常用于标记版本发布点(例如v1.0, v2.0).  
  - 标签分为轻量标签(`lightweight tag`)和附注标签(`annotated tag`)两种, 其中`annotated`类型的标签可以记录提交信息, 轻量级标签只保存提交哈希值, 轻量级标签创建后无法修改, 但`annotated`标签可以修改

```bash
# 列出所有标签
git tag

# 创建轻量标签(lightweight tag), 对提交的引用, 不包含额外的元数据(创建者, 日期, 描述等)
git tag <标签名>

# 创建附注标签(annotated tag), 包含创建者, 日期, 注释, 签名等元数据, 适合正式版发布
git tag -a <tagname> -m "<message>"

# 为指定提交创建标签, 可以是之前的提交
git tag <tagname> <commit-hash>

# 查看标签的详细信息(创建者, 日期, 注释)
git show <tagname>

# 删除本地仓库标签
git tag -d <tagname>

# 删除远程仓库的标签, 须提前删除本地标签, 再推送删除操作到远程仓库
git tag -d <tagname>
git push origin :refs/tags/<tagname>

# 推送标签到远程仓库, 默认标签不会自动推送到远程仓库
git push origin <tagname>

# 推送所有标签到远程仓库
git push origin --tags

```

- 拉取

获取远程仓库的更新并自动和当前分支合并, 先运行"`git fetch`"获取最新的代码变更, 再运行"`git merge`"将远程的改动合并到当前分支

```bash
# 拉取远程仓库最新更新到本地分支
git pull

# 指定远程仓库和分支进行拉取
git pull <remote> <branch>

# 拉取时使用"--rebase"而非合并, 可避免产生合并提交(merge commit)
git pull --rebase origin main

# 拉取指定标签
git pull origin tag <tagname>

# 只拉取最新更新, 不进行合并
git fetch

# 将远程的更改强制覆盖本地的所有改动
git pull --hard

```

- 拉取(不合并)

`git fetch` 用于从远程仓库获取更新的分支和提交记录, 但不会自动将这些更改合并到本地工作分支(不改变本地代码). 它与`git pull`不同, `git fetch`只是获取数据, 允许你手动决定如何处理这些更新. 它常用于查看远程仓库的变化, 而不影响当前的开发工作.

```bash
# 获取远程仓库的更新, 但不会自动将这些更改合并到本地工作分支
git fetch

# 获取指定远程仓库的更新
git fetch <远程仓库名>

# 获取指定分支
git fetch origin <分支名>

# 强制同步远程引用, 当本地引用与远程分支存在冲突时, 强制更新
git fetch --force

# 仅获取新标签
git fetch --tags

# 移除本地无效远程引用
git fetch --prune

```


- 推送

  - `git push` 是将本地分支的提交推送到远程仓库的操作. 它可以把本地的提交记录同步到远程仓库, 通常用于将代码, 文档或项目的更新共享给团队成员. `git push` 会更新远程仓库中相应分支的状态, 使远程仓库的代码库和本地保持一致.
  - 执行前`git push`前, 先运行`git pull`或`git fetch`同步远程仓库的最新改动, 确保本地仓库的最新状态.
  - 使用 `git rebase` 重写提交历史后, 需要强制推送. 执行 `git rebase` 操作时一定要确保你不会修改已经分享给其他协作者的提交历史, 以避免团队协作中的混乱.
  - `git push`不会默认推送标签, 标签需要通过 `git push origin <tag>` 或 `git push --tags` 单独推送. 如果没有推送标签, 其他人将无法获取到你的标签更新.

```bash
# 推送当前分支到默认远程仓库
git push

# 推送分支到指定远程仓库和分支
git push <remote> <branch>

# 推送所有本地分支到远程仓库
git push --all <remote>

# 推送标签到远程仓库
git push <remote> <branch>

# 推送所有标签
git push --tags

# 强制推送, 强制推送会覆盖远程仓库中相应分支的提交，适用于历史记录需要重写的情况
git push origin main --force

# 推送时设置上游分支, 如果本地分支还没有设置远程追踪分支, 第一次推送时可以设置上游分支
git push -u <remote> <branch>

# 删除远程分支
git push origin --delete <分支名>

```

- 分支

管理分支, 允许同时进行多个分支任务, 使用"`git branch -a`"查看所有分支和远程分支, 删除时须确认已合并

```bash
# 查看本地分支
git branch

# 查看远程分支
git branch -r

# 查看本地和远程所有分支
git branch -a

# 创建新分支
git branch <新分支名>

# 删除本地分支
git branch -d <分支名>
# 强制删除本地分支(未合并时)
git branch -D <分支名>

# 重命名当前分支
git branch -m <分支名>

```

- 分支切换

`git checkout`不止用于切换分支, 还可以切换文件版本或创建新分支  
`git switch`专注于切换和创建分支, 使用`git restore`恢复文件

```bash
# 切换已有分支(new)
git switch <分支名>

# 强制切换分支(可能丢失未提交的更改)
git switch -f <分支名>

# 创建并切换新分支(new)
git switch -c <分支名>
git switch -c feature/new-feature

#  指定起点(提交或分支)创建并切换到新分支
git switch -c <新分支> <起点>

#切换至最近一次所在分支
git switch -

# 恢复未追踪的工作区改动(不覆盖未提交的更改)
git switch --recurse-submodules <分支名>

# 切换已有分支(old)
git checkout <分支名>

# 切换并创建新分支(old)
git checkout -b <分支名>

```

- 合并

合并两个分支的修改, 合并时会产生冲突, 需手动解决冲突, 合并前建议"`git pull`"或"`git fetch`"确保分支是最新状态, 提前切换到目标分支, 再合并被合并分支

```bash
# 合并分支
git merge <被合并分支>

# 查看合并历史
git log --merges

# 快速前进合并(当目标分支是被合并分支的祖先时, 会进行快速前进合并, 即不创建新的合并提交, 而是直接将目标分支指向被合并分支)
git merge <分支名>

# 禁止快速前进合并(强制创建一个合并提交)
git merge --no-ff <分支名>

# 终止合并
git merge --abort

# 合并冲突(当两个分支合并同一部分有不同修改时, 会产生冲突, 需要手动修改标记区域, 并重新提交)
<<<<<< HEAD
当前分支代码
=======
被合并分支代码
>>>>>> branch

```
