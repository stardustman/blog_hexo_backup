---
title: master-git
date: 2019-12-28 13:15:29
tags: ["git"]
---

## git init

> Create an empty Git repository or reinitialize an existing one
> 也即是向 `working directory` 里添加 `.git` 文件夹

```
.git/
├── branches # git 分支
├── config   # git 配置
├── description
├── HEAD     # HEAD 指针, 指向当前分支最新的 commit
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── info
│   └── exclude
├── objects   # 存放不同种类的 git 对象，实现版本管理的关键 
│   ├── info
│   └── pack
└── refs
    ├── heads 
    └── tags  # git 标签

```

## 第一次 git add | git commit
### git add

> Add file contents to the index
> 添加文件内容到 `index` 区

1. 添加两个文件 `file1` 和 `file2`, 文件内容分别是 `111` 和 `222`

2. git add file_name
```sh
git add file1
```

3. tree .git/objects 查看添加的 git object
```
.git/objects/
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── info
└── pack
```
> 对 file1 文件内容也就是 `111` 和一些其他的信息做 `SHA-1` 运算得出 `58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c` 这个 `40` 位 `16 进制`的值. 

4. git cat-file 查看 objects 相关信息
**查看 object 类型** 
```sh
git cat-file -t 58c9bd
blob
```

**查看 object 大小** 
```sh
git cat-file -s 58c9bd
4
```

**查看 object 内容** 
```sh
git cat-file -p 58c9bd
111
```
5. 对文件内容为 `222` 的 `file2` 进行步骤 2 ~ 4.

查看 .git/objects 如下:
```sh
.git/objects/
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c # file1:blob
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c # file2:blob
├── info
└── pack
```

### git commit

> Stores the current contents of the index in a new commit along with a log message from the user describing the changes.

#### 执行 git commit -m "add file1 and file2"

#### 查看 .git/objects 的变化
```sh
tree .git/objects
.git/objects/
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c # file1:blob
├── 73
│   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5 # 1:commit
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c # file2:blob
├── d9
│   └── 64f468f776fac72a049a884ae24e7dbb838fd0 # 1:tree
├── info
└── pack
```

### 查看 HEAD
```sh
root@aliyun:~/blog# cat .git/HEAD
ref: refs/heads/master # 目前在 master 分支上
root@aliyun:~/blog# cat .git/refs/heads/master
73622e96d5b5389d63dd74e3c40db9d4c4296ff5 # 1:commit 
```

## 第二次 git add | git commit

> 修改 `file1` 的文件内容为 `333`

### git add file1

```sh
root@aliyun:~/blog# git add file1
root@aliyun:~/blog# tree .git/objects/
.git/objects/
├── 55
│   └── bd0ac4c42e46cd751eb7405e12a35e61425550 # file1_modify: blob
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── 73
│   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
├── d9
│   └── 64f468f776fac72a049a884ae24e7dbb838fd0
├── info
└── pack
```

### git commit -m "modify file1"

```sh
root@aliyun:~/blog# git commit -m "modify file1"
[master 558e28c] modify file1
 1 file changed, 1 insertion(+), 1 deletion(-)
root@aliyun:~/blog# tree .git/objects/
.git/objects/
├── 55
│   ├── 8e28c67089eea9bdfbfd748d32baa8f2ce944e # 2:commit
│   └── bd0ac4c42e46cd751eb7405e12a35e61425550 # file1_modify: blob
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── 61
│   └── 74c3edb1e8dc4f1692b69fd43c8c6af85ce560 # 2:tree
├── 73
│   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
├── d9
│   └── 64f468f776fac72a049a884ae24e7dbb838fd0
├── info
└── pack
```

### 查看 HEAD

```sh
root@aliyun:~/blog# cat .git/HEAD
ref: refs/heads/master
root@aliyun:~/blog# cat .git/refs/heads/master
558e28c67089eea9bdfbfd748d32baa8f2ce944e # 指向了第二次 commit 的 hash 值
```
### git add | git commit 实质

git add: 向 `.git/objects` 添加类型是 `blob` 的文件
git commit: 向 `.git/objects` 添加类型是 `tree` 和 `commit` 的文件

## git branch

>  List, create, or delete branches

### 创建一个 `new-branch` 的分支
```sh
root@aliyun:~/blog# git branch new-branch
root@aliyun:~/blog# tree .git/
.git/
├── branches
├── COMMIT_EDITMSG
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           ├── master
│           └── new-branch # git branch new-branch 添加的分支日志
├── objects
│   ├── 55
│   │   ├── 8e28c67089eea9bdfbfd748d32baa8f2ce944e
│   │   └── bd0ac4c42e46cd751eb7405e12a35e61425550
│   ├── 58
│   │   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
│   ├── 61
│   │   └── 74c3edb1e8dc4f1692b69fd43c8c6af85ce560
│   ├── 73
│   │   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5
│   ├── c2
│   │   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
│   ├── d9
│   │   └── 64f468f776fac72a049a884ae24e7dbb838fd0
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   ├── master
    │   └── new-branch # git branch new-branch 添加的 refs
    └── tags

18 directories, 29 files
```

### git branch - -list 查看分支

```sh
root@aliyun:~/blog# git branch --list
* master # * 表示当前所在分支
  new-branch # 新创建的 new-branch 分支
```
### git checkout new-branch 切换分支

```sh
root@aliyun:~/blog# git checkout new-branch
Switched to branch 'new-branch'
root@aliyun:~/blog# git branch --list
  master
* new-branch
```
创建分支 `new-branch`, 并切换到 `new-branch` 分支.
1. git branch new-branch
2. git checkout new-branch
可以简化为:
```sh
git checkout -b new-branch
```

### 查看 HEAD

```sh
root@aliyun:~/blog# cat .git/HEAD
ref: refs/heads/new-branch
root@aliyun:~/blog# cat .git/refs/heads/new-branch
558e28c67089eea9bdfbfd748d32baa8f2ce944e # 指向了第二次 commit 的 hash值
```
由此可知 git branch new-branch 创建一个分支,实际上做了两件事情:
1. 添加 .git/logs/refs/heads/new-branch 记录该分支的 `log`
2. 添加 .git/refs/heads/new-branch 指向最新的 `commit`
这也就是 `git` 创建新的分支如此快的原因. 

## 第三次 git add | git commit

切换到 `new-branch` 分支, 新建一个 `file3` 的文件, 内容是 `new-branch-file3`

### git add file3
```sh
root@aliyun:~/blog# git add file3
root@aliyun:~/blog# tree .git/objects/
.git/objects/
├── 2e
│   └── 799a48d8c045b814c3862af40dea98bdde790a # file3
├── 55
│   ├── 8e28c67089eea9bdfbfd748d32baa8f2ce944e
│   └── bd0ac4c42e46cd751eb7405e12a35e61425550
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── 61
│   └── 74c3edb1e8dc4f1692b69fd43c8c6af85ce560
├── 73
│   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
├── d9
│   └── 64f468f776fac72a049a884ae24e7dbb838fd0
├── info
└── pack

9 directories, 8 files
root@aliyun:~/blog# git cat-file -t 2e79
blob
root@aliyun:~/blog# git cat-file -p 2e79
new-branch-file3
root@aliyun:~/blog# git cat-file -s 2e79
17

```
### git commit -m "file3 on new-branch"

```sh
root@aliyun:~/blog# git commit -m "add file3 on new-branch"
[new-branch ce907fa] add file3 on new-branch
 1 file changed, 1 insertion(+)
 create mode 100644 file3
root@aliyun:~/blog# tree .git/objects/
.git/objects/
├── 2e
│   └── 799a48d8c045b814c3862af40dea98bdde790a
├── 55
│   ├── 8e28c67089eea9bdfbfd748d32baa8f2ce944e
│   └── bd0ac4c42e46cd751eb7405e12a35e61425550
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── 61
│   └── 74c3edb1e8dc4f1692b69fd43c8c6af85ce560
├── 73
│   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5
├── b7
│   └── 2118da210e8e3f4127cfc16cc819eff58e42e3 # new-branch:tree
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
├── ce
│   └── 907fabd08138058b05428d7cac017507b33c44 # new-branch:commit
├── d9
│   └── 64f468f776fac72a049a884ae24e7dbb838fd0
├── info
└── pack

11 directories, 10 files
root@aliyun:~/blog# git cat-file -t b721
tree
root@aliyun:~/blog# git cat-file -p b721 # 全量的快照(snapshot)
100644 blob 55bd0ac4c42e46cd751eb7405e12a35e61425550    file1
100644 blob c200906efd24ec5e783bee7f23b5d7c941b0c12c    file2
100644 blob 2e799a48d8c045b814c3862af40dea98bdde790a    file3
root@aliyun:~/blog# git cat-file -s b721
99
root@aliyun:~/blog# git cat-file -t ce90
commit
root@aliyun:~/blog# git cat-file -p ce90
tree b72118da210e8e3f4127cfc16cc819eff58e42e3
parent 558e28c67089eea9bdfbfd748d32baa8f2ce944e # 上一次 commit 
author root <root@aliyun> 1578039532 +0800
committer root <root@aliyun> 1578039532 +0800

add file3 on new-branch # commit message
root@aliyun:~/blog# git cat-file -s ce90
208

```

### 查看 HEAD

```sh
root@aliyun:~/blog# cat .git/HEAD
ref: refs/heads/new-branch # 当前在 new-branch 分支上
root@aliyun:~/blog# cat .git/refs/heads/new-branch
ce907fabd08138058b05428d7cac017507b33c44 # new-branch 分支最新的提交

```

### 切换到 master 分支查看 HEAD
```sh
root@aliyun:~/blog# git checkout master
Switched to branch 'master'
root@aliyun:~/blog# cat .git/HEAD
ref: refs/heads/master # 当前在 master 分支
root@aliyun:~/blog# cat .git/refs/heads/master
558e28c67089eea9bdfbfd748d32baa8f2ce944e # master 分支最新的提交
```
查看 `master` 和 `new-branch` 两个分支可知, 每一分支对应的 `HEAD` 和 `.git/refs/heads/branch_name` 记录的 `commit` 不同. 这就是 git 分支的实现.

## git add | git commit | git branch 图解
{% asset_img git.svg  git %}

## Fast-forward Merge
> 两次 git add 和 git commit 之后，创建 `new-branch` ，在 `new-branch` 进行了修改。
> 但是 `master` 分支没有做任何更改。也就是 `master` 此时分支落后了 `new-branch` 分支。
> `new-branch` 分支的修改 `master` 分支怎样看到呢？可以进行所谓的 `Fast-forward merge`。

### 切换到 master 分支
```
root@aliyun:~/blog# git checkout master  # 切换到 master 分支
Switched to branch 'master'
root@aliyun:~/blog# cat .git/refs/heads/master # master 分支仍然指向第二次的 commit。
558e28c67089eea9bdfbfd748d32baa8f2ce944e
root@aliyun:~/blog# tree .git/objects/ # 查看 objects 对象。
.git/objects/
├── 2e
│   └── 799a48d8c045b814c3862af40dea98bdde790a
├── 55
│   ├── 8e28c67089eea9bdfbfd748d32baa8f2ce944e
│   └── bd0ac4c42e46cd751eb7405e12a35e61425550
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── 61
│   └── 74c3edb1e8dc4f1692b69fd43c8c6af85ce560
├── 73
│   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5
├── b7
│   └── 2118da210e8e3f4127cfc16cc819eff58e42e3
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
├── ce
│   └── 907fabd08138058b05428d7cac017507b33c44
├── d9
│   └── 64f468f776fac72a049a884ae24e7dbb838fd0
├── info
└── pack

11 directories, 10 files
```

### 合并 new-branch 分支到 master 分支

```
root@aliyun:~/blog# git merge new-branch # 合并 new-branch 分支
Updating 558e28c..ce907fa ### 更新 558e 这个object
Fast-forward # fast-forward
 file3 | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 file3
root@aliyun:~/blog# ls
file1  file2  file3
root@aliyun:~/blog# tree .git/objects/ # 查看 objects 对象。并没增加任何 object
.git/objects/
├── 2e
│   └── 799a48d8c045b814c3862af40dea98bdde790a
├── 55
│   ├── 8e28c67089eea9bdfbfd748d32baa8f2ce944e
│   └── bd0ac4c42e46cd751eb7405e12a35e61425550
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── 61
│   └── 74c3edb1e8dc4f1692b69fd43c8c6af85ce560
├── 73
│   └── 622e96d5b5389d63dd74e3c40db9d4c4296ff5
├── b7
│   └── 2118da210e8e3f4127cfc16cc819eff58e42e3
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
├── ce
│   └── 907fabd08138058b05428d7cac017507b33c44
├── d9
│   └── 64f468f776fac72a049a884ae24e7dbb838fd0
├── info
└── pack

11 directories, 10 files
root@aliyun:~/blog# cat .git/refs/heads/master 
ce907fabd08138058b05428d7cac017507b33c44 # 更新了 master 指向，指向了 new-branch 的 commit。

```

### Fast-Forward 图解
{% asset_img fast-forward-merge.svg  git fast forward %}

## Reference
1. [git-under-the-hood](https://www.lzane.com/slide/git-under-the-hood/index.html#/)
2. [Bilibili-git-under-the-hood](https://www.bilibili.com/video/av77252063)
3. [SHA-1](https://zh.wikipedia.org/wiki/SHA-1)
4. [git-branch](https://learngitbranching.js.org/)
5. [visual-git-reference](http://marklodato.github.io/visual-git-guide/index-en.html)
