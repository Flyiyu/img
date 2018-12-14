

### Git的使用

- Git与提交有关的三个命令对应的操作，Add命令是把文件从IDE的工作目录添加到本地仓库的stage区，Commit命令把stage区的暂存文件提交到当前分支的仓库，并清空stage区。Push命令把本地仓库的提交同步到远程仓库。

- 获取更新有两个命令：Fetch和Pull，Fetch是从远程仓库下载文件到本地的origin/master，然后可以手动对比修改决定是否合并到本地的master库。Push则是直接下载并合并。

- 创建分支

  ![图片](https://img-blog.csdn.net/20160912171844429)

  选择New Branch并输入一个分支的名称

​     ![图片2](https://img-blog.csdn.net/20160912171858663)



git的基本工作流程：

[![git_status](http://img2.tbcdn.cn/L1/461/1/ac6d9422c3f843017b2441b212f39ebc00697e4c)](http://img2.tbcdn.cn/L1/461/1/ac6d9422c3f843017b2441b212f39ebc00697e4c)

- git clone：将远程的Master分支代码克隆到本地仓库
- git checkout：切出分支出来开发
- git add：将文件加入库跟踪区
- git commit：将库跟踪区改变的代码提交到本地代码库中
- git push： 将本地仓库中的代码提交到远程仓库

git 分支

- 主分支
  - master分支：存放随时可供生产环境中的部署的代码
  - develop分支：存放当前最新开发成果的分支，当代码足够稳定时可以合并到master分支上去。
- 辅助分支
  - feature分支：开发新功能使用，最终合并到develop分支或抛弃掉
  - release分支：做小的缺陷修正、准备发布版本所需的各项说明信息
  - hotfix分支：代码的紧急修复工作

## 分支合并

现在我们要把 dev-100 分支上的代码合并到 master 主分支上
先切换到 master 分支
![img](http://img12345.5-project.com/blog/20181102172230.png)

合并 dev-100 分支到 master 分支之前，建议先对 master 代码进行 pull 更新操作，然后再执行 Merge into Current



![img](http://img12345.5-project.com/blog/20181102172650.png)

``` 说明：
【new branch】新建分支
【local branches】本地分支
【master】表示当前是主分支
【remote branches】远程仓库分支。我在这里配置了两个远程仓库，所以这里显示2个。
```



如果没有冲突，dev-100 中的代码就会被合并到 master 分支上了，合并成功后，需要 `push` 才能推送到远程仓库,一般情况下只需要将分支提交到本地仓库，不需要将分支提交远程仓库。如果将所有的分支都提交到远程仓库，会让远程仓库杂乱无章。
![img](http://img12345.5-project.com/blog/20181102171807.png)

## 取消分支合并

合并完成后，但是由于一些问题，我们想要取消本次合并，右键 git，选择 Reset HEAD
![img](http://img12345.5-project.com/blog/20181102173946.png)
![img](http://img12345.5-project.com/blog/20181102174125.png)

HEAD^ 是还原到上一个版本，HEAD^^ 是还原到上上一个版本。
Reset Type 有三种：

- mixed 默认方式，只保留源码，回退commit和index信息
- soft 回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit
- hard 彻底回退，本地源码也会变成上一个版本内容

一般使用默认的 mixed 或者粗暴的 hard 方式。
我们这里是取消合并，所以选择 **Hard** 方式，并且是**HEAD^**还原到上一个版本，回退后恢复了原来 master 的代码。
![img](http://img12345.5-project.com/blog/20181102171518.png)

## 解决合并冲突问题

接下来演示合并冲突，此时是在 master 分支，我们修改文件，并 commit 以及 push 到远程仓库。
![img](http://img12345.5-project.com/blog/20181102174704.png)

此时再把 dev-100 分支合并到 master 分支就会提示冲突。
![img](http://img12345.5-project.com/blog/20181102175031.png)

双击冲突文件，处理冲突。
![img](http://img12345.5-project.com/blog/20181102175217.png)
处理完成后，点击 apply 即可，如果有多个冲突文件，都按照这种方式处理，这是我们处理完冲突之后的代码。
![img](http://img12345.5-project.com/2018110315412022049329.png)

dev-100 分支已经被成功合并到 master 了，就可以删除了。可以直接删除远程 dev-100 分支，删除时 IDEA 会提示是否同时删除本地的 dev-100 分支，勾选即可。

现在我们把分支合并的结果 push 到远程仓库。

## 代码暂存之git stash

编号 100 的需求完成之后，现在我们又接到一个新的需求，正在 dev-101 分支进行开发，开发还未完成。
![img](http://img12345.5-project.com/blog/20181102180114.png)

突然线上出现 bug，需要我们紧急进行修改，于是我们要基于最新的 master 分支新建一个 bug 分支 bug-12，需要先切换到 master 分支，但是当前分支的代码没有commit， 如果直接切换到 master 分支的话，dev-101 分支上的新增代码就会跑到 master 分支，而代码又不能此时 commit ，于是就轮到 stash 出场了。
![img](http://img12345.5-project.com/blog/20181102180427.png)
Stash 会保存当前工作进度，会把暂存区和工作区的改动保存起来。
![img](http://img12345.5-project.com/20181103154120204424605.png)
添加备注，选择 **CREATE STASH**。你会发现当前工作区内的代码被恢复成了原样。
![img](http://img12345.5-project.com/2018110315412022049329.png)

## 代码暂存还原

此刻切换到 master 分支，并创建 bug-12 分支进行修复 bug，修复完成后合并到 master 分支并 push 到远程仓库，上文已经演示如何合并，在此不再赘述。

将 bug-12 与 master 合并完成之后，现在要接着写 dev-101 需求代码，首先先切换到 dev-101 分支；
但是之前的代码已经被我们放到了 git 的 stash 当中，我们现在要把代码还原到工作区当中。
选择 Unstash Changes
![img](http://img12345.5-project.com/20181103154120268155941.png)
![img](http://img12345.5-project.com/20181103154120276883579.png)
选择之前保存的，同时勾选 Pop stash（还原完成后，会自动删除这个 stash），确定后，工作区之前写的代码就又回来了。
![img](http://img12345.5-project.com/blog/20181102180114.png)

