
# 😉Git分布式版本控制工具

> Git工作流程图

![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816155907836-1566095029.png)

```bash
clone (克隆): 从远程仓库中克隆代码到本地仓库

checkout (检出) : 从本地仓库中检出一个仓库分支然后进行修订

add (添加): 在提交前先将代码提交到暂存区

commit (提交): 提交到本地仓库。本地仓库中保存修改的各个历史版本

fetch (抓取) : 从远程库，抓取到本地仓库，不进行任何的合并动作，一般操作比较少

pull (拉取) : 从远程库拉到本地库，自动进行合并(merge),然后放到到工作区，相当于 fetch + merge

push (推送) : 修改完成后，需要和团队成员共享代码时，将代码推送到远程仓库

```

```bash


# 配置别名简化命令

# 打开用户目录，创建 .bashrc 文件

# 部分windows系统不允许用户创建点号开头的文件，可以打开gitBash,执行 touch ~/.bashrc

# 在 .bashrc 文件中输入如下内容

# 用于输出git提交日志
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'

#用于输出当前目录所有文件及基本信息
alias ll='ls -al'

---

# 解决GitBash乱码问题

# 1.打开GitBash执行下面命令

git config --global core.quotepath false

# 2.${git_home}/etc/bash.bashrc 文件最后加入下面两行

export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"

```

![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816162830665-2125005324.png)

## 👺Git命令

```bash

cat 文件名 # 查看文件内容

touch 文件名 # 创建文件

```

### 设置用户签名

```

git config --global user.name

git config --global user.email

```

### 初始化本地库

```bash

# 在工作目录执行
git init

```

### ️查看本地库状态

```

git status

```

### 添加暂存区

```bash

# 添加到暂存区
git add 文件名  

# 将所有修改加入暂存区
git add . 

# 从暂存区删除
git rm --cached <file>...

```

### 提交到本地库

```bash

git commit -m "日志信息" 文件名

```

### 查看提交日志

```bash

git reflog # 可以查看已经删除的提交记录

git log # 详细信息

git log [option]

```


```bash

option 包括 

--all # 显示所有分支

--pretty=oneline # 将提交信息显示为一行

--abbrev-commit # 使得输出的 commit 更简短

--graph # 以图的形式显示

```



### .gitignore

有些文件不需要交给git管理

可以创建一个 `gitignore` 文件,列出要忽略的文件模式

忽略文件如何阅读,常见格式

```yml

# 所有以.a 结尾的文件讲被忽略(递归)
*.a
# 不管其他规则怎样,强制不忽略  lib.a
!lib.a
# 只忽略 文件 TODO (注意这里是文件)
/TODO
# 忽略 build文件夹下所有内容(递归) 这里是文件夹
build/
# 忽略 doc 目录下以 *.txt 结尾的文件 (不递归)
doc/*.txt
# 忽略 doc 目录下以 *.pdf 结尾的文件 (递归)
doc/**/*.pdf

```

### 版本穿梭

```bash

git reset --hard 版本号

```

### 分支操作

```bash

git branch 分支名 # 创建分支

git branch -v  # 查看分支

git checkout 分支名  # 切换分支

git checkout -b 分支名 # 创建并切换

git merge 分支名  # 把指定的分支合并到当前分支上

git branch -d 文件名 删除分支时,需要做各种检查

git branch -D 文件名 不做任何检查,强制删除

```

### 合并分支冲突

> 当两个分支上对文件的修改可能会存在冲突,列如:同时修改了同一个文件的同一行,这时就需要手动解决冲突,步骤如下:
>
> - 处理文件冲突的地方
>
> - 将解决完冲突的文件加入暂存区(add)
>
> - 提交到仓库(commit)

![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816160712360-1135700266.png)

### 规范

> 几乎所有的版本控制系统都已某种形式支持分支.
> 
> 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修复,开发新功能,以免影响开发主线.

> 在开发中,一般有如下分支使用规范与流程
>
> master (生产) 分支
>
> develop (开发) 分支
>
> feature/xxxx分支
>
> 从develop创建的分支,一般是同期并行开发,但不同期上线时创建的分支,完成后合并到develop分支
>
> hotfix/xxxx分支
>
> 从master派生的分支,一般作为线上bug修复,修复完需要合并到master,test,develop分支.
>
> 还有一些分支,例如: test分支(用于代码测试) ,pre分支(预上线分支)等等

![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816161041918-1142955935.png)

### 🥵创建远程仓库

![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816170922798-389286129.png)

> 配置SSH公钥

- 生成SSH公钥

  - ssh-keygen -t rsa

  - 不断回车

    - 如果公钥已经存在，则自动覆盖

- Gitee设置账户共公钥

  - 获取公钥

    - cat ~/.ssh/id_rsa.pub

  - ![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816171224002-638444431.png)

  - 验证是否配置成功

    - ssh -T git@gitee.com

### 🪶操作远程仓库

> 添加远程仓库

此操作是先初始化本地库，然后与已创建的远程库进行对接

- 命令： git remote add <远端名称> <仓库路径>

  - 远端名称，默认是origin，取决于远端服务器设置

  - 仓库路径，从远端服务器获取此URL

  - 例如: git remote add origin git@gitee.com:czbk_zhang_meng/git_test.git

    ![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816171521243-1088146687.png)

> 查看远程仓库

- 命令：git remote

    ![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816171624447-1502870330.png)

> 推送到远程仓库

- 命令：git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名] ]

  - 如果远程分支名和本地分支名称相同，则可以只写本地分支

  - git push origin master

  - -f 表示强制覆盖

  - --set-upstream 推送到远端的同时并且建立起和远端分支的关联关系。

  - git push --set-upstream origin master

  - 如果当前分支已经和远端分支关联，则可以省略分支名和远端名。

    - git push 将master分支推送到已关联的远端分支。

    ![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816171919838-209383161.png)


>  本地分支与远程分支的关联关系

- 查看关联关系我们可以使用 git branch -vv 命令

    ![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816172210756-341264465.png)

> 从远程仓库克隆

如果已经有一个远端仓库，我们可以直接clone到本地。

- 命令: git clone <仓库路径> [本地目录]

  - 本地目录可以省略，会自动生成一个目录

![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816172322002-771757415.png)

> 从远程仓库中抓取和拉取
>
> 远程分支和本地的分支一样，我们可以进行merge操作，只是需要先把远端仓库里的更新都下载到本地，再进行操作。

- 抓取 命令：git fetch [remote name] [branch name]

  - 抓取指令就是将仓库里的更新都抓取到本地，不会进行合并

  - 如果不指定远端名称和分支名，则抓取所有分支。

- 拉取 命令：git pull [remote name] [branch name]

  - 拉取指令就是将远端仓库的修改拉到本地并自动进行合并，等同于fetch+merge

  - 如果不指定远端名称和分支名，则抓取所有并更新当前分支。

---

1. 在test01这个本地仓库进行一次提交并推送到远程仓库

    ![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816172639602-827994769.png)

2. 在另一个仓库将远程提交的代码拉取到本地仓库

    ![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816172719465-1891600536.png)

### 🥲解决合并冲突

> 在一段时间，A、B用户修改了同一个文件，且修改了同一行位置的代码，此时会发生合并冲突。
> 
> A用户在本地修改代码后优先推送到远程仓库，此时B用户在本地修订代码，提交到本地仓库后，也需要推送到远程仓库，此时B用户晚于A用户，故需要先拉取远程仓库的提交，经过合并后才能推送到远端分支,如下图所示。

![img](https://img2023.cnblogs.com/blog/3134450/202308/3134450-20230816172838490-219635838.png)

> 在B用户拉取代码时，因为A、B用户同一段时间修改了同一个文件的相同位置代码，故会发生合并冲突。
> 
> 远程分支也是分支，所以合并时冲突的解决方式也和解决本地分支冲突相同相同.


