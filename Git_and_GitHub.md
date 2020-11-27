#### 分布式版本控制系统   (Git)

在管理项目时存放的不是项目版本与项目版本之间的差异，而是***索引***

客户端并***不只提取最新版本***的文件快照，而是把整个代码仓库完整的镜像下来

#### 本地结构：

*本地库：*存放历史版本的地方

*暂存区:*  临时存放代码地方，到本地库  git commit

*工作区：*写代码的地方  到暂存区git add



### 同团队合作

远程库：在代码托管中心创建的，本地库到远程库用push命令；远程库到本地库用clone(职员)和pull(项目经理)命令

### 跨团队合作

A公司的项目经理将本地库推到远程库，然后B公司的职员需要将A的远程库进行fork操作复制一份形成一个新的远程库。

B的远程库更新之后需要pull request 拉取申请，然后A的远程库进行审核，审核通过则merge合并



#### 远程库：

局域网：GitLab

外网： GitHub，Gitee   现成的托管中心



## 初始化本地仓库：

1、创建一个文件夹   （此时Git并不知道要把这个文件夹作为你的本地仓库）

2、打开GitBash终端

3、设置签名：设置用户名和邮箱

```c
$ git config --global user.name "DZH"                  //设置用户名
$ git config --global user.email "2247677474@qq.com"   //设置邮箱

```

4、本地仓库的初始化

```c
$ git init
//Initialized empty Git repository in F:/桌面/GitResp/.git/      //结果
```

## 添加文件add和提交文件commit

1、先创建一个要提交的文件（工作区）

2、将文件提交到暂存区           

 ```c
$ git add Demo.txt
 ```

3、将文件提交到本地库

```C
$ git commit -m "提交的第一个测试文件 Demo.txt" Demo.txt    //-m "" 是注释信息

[master (root-commit) fc409c0] 提交的第一个测试文件 Demo.txt
 1 file changed, 1 insertion(+)
 create mode 100644 Demo.txt


```

注意：不放在本地仓库中的文件，Git是不进行管理的

​           放在本地仓库中的文件，Git也不管理，必须通过add和commit命令操作

## 查看工作区和暂存区的状态       git   status

```c
$ git status

On branch master                                      //在主分支上
nothing to commit, working tree clean                 //没有文件要提交


```

如果存在未提交的文件时

```c
$ git status

On branch master
Untracked files:                                      //未追踪的文件 Demo2.txt
  (use "git add <file>..." to include in what will be committed)
        Demo2.txt

nothing added to commit but untracked files present (use "git add" to track)

```

如果已经提交了文件，然后再修改了该文件之后查看状态

```c
$ git status

On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   Demo.txt                           //显示修改了Demo.txt,是红色

no changes added to commit (use "git add" and/or "git commit -a")

```

## 查看提交的，显示从最近到最远的日志    git   log

```C
$ git log
                                                                  //理解为键值对
commit 952df653061889b94f383bc107bf2293cba65bf2 (HEAD -> master)  //key
Author: DZH <2247677474@qq.com>                                   //value
Date:   Thu Nov 26 20:18:09 2020 +0800

    修改了Demo.txt文件内容

commit fc409c0dccdb27c7ab79c87ebe9fbd3060ffe011
Author: DZH <2247677474@qq.com>
Date:   Thu Nov 26 20:03:55 2020 +0800

    提交的第一个测试文件 Demo.txt

//越往下是越早的版本信息
//当历史记录太多时，查看日志会有分页的效果，空格下一页，b上一页，到尾页会显示END，q退出
//git log --pretty = oneline                //一行显示一条历史记录的方式查看
//git log --oneline                         //上面方式的简洁，将索引截取了前面一部分
//git reflog           //上面方式多了HEAD@{数字}  数字代表指针回到当前历史需要走多少步
```

## 前进或后退到历史版本  git  reset

--hard          (本地库的指针移动的同时，重置暂存区，重置工作区)

```c
/**************当前查看****************************************************/
$ git reflog
952df65 (HEAD -> master) HEAD@{0}: commit: 修改了Demo.txt文件内容
fc409c0 HEAD@{1}: commit (initial): 提交的第一个测试文件 Demo.txt
/**************回退到HEAD@{1}对应的索引号fc409c0****************************/
$ git reset --hard fc409c0
/**************回退之后查看************************************************/
 $ git reflog
fc409c0 (HEAD -> master) HEAD@{0}: reset: moving to fc409c0  //指针移到了fc409c0
952df65 HEAD@{1}: commit: 修改了Demo.txt文件内容
fc409c0 (HEAD -> master) HEAD@{2}: commit (initial): 提交的第一个测试文件 Demo.txt
//此时可以查看Demo.txt，发现已经回到了之前的状态 
```

--mixed          (本地库的指针移动的同时，重置暂存区，但是工作区不动)   （*不太常用*）

--soft              (本地库的指针移动的同时，但是暂存区不动，工作区也不动)   （*不太常用*）

## 删除文件，找回本地库中删除的文件

```c
 rm Test.txt
```

这只是将工作区的Test文本删除，而暂存区和本地库并没有删除，如果要将删除操作同步到暂存区和本地库，直接在进行add和commit操作即可

## 文件比对  diff

在工作区建立了一个文本，然后提交到暂存区，提交到本地库，最后我又更新了工作区的文本内容，此时工作区与暂存区的文本内容不一致，可以用diff命令查看对比哪里不一致

```c
$ git diff Test.txt
diff --git a/Test.txt b/Test.txt
index 4632e06..5682420 100644
--- a/Test.txt
+++ b/Test.txt
@@ -1 +1,3 @@
-123456                              //先删除原来的
\ No newline at end of file
+123456                             //在添加
+
+aaaabbbb
\ No newline at end of file
    
//多个文件比对时，直接git diff
```

如果是add之后，则暂存区与本地库的内容不一致，如果需要对比暂存区与本地库的内容，则使用命令：git diff  索引号/HEAD Test.txt

# 分支

在版本控制过程中，使用多条线同时推进多个任务，这里的多条线，就是多个分支

好处：同时多个分支并行开发，互不耽误，互不影响，提高开发效率，如果有一分支开发失败，直接删掉这个分支就可以了，不会影响其他分支。

## 查看分支、创建分支、切换分支

```c
$ git branch -v                         //查看分支命令
* master 8744b0c 提交了Test文件          //显示分支的最新版本   *代表此时在此分支
```

```c
$ git branch branch01                   //创建一个名为branch01的分支命令
```

```c
$ git checkout branch01                 //切换到branch01分支命令
```

## 分支冲突问题

在主分支里面提交了一个文件BranchTxt，然后我又创建了一个新分支branch01。此时，我在工作区更新了BranchTxt的内容，我将其提交到主分支上进行版本更新，随后我又在工作区更改了BranchTxt的内容，将其提交到了branch01分支上进行版本更新，此时两个分支上的两个更新版本是互不相干的。现在我需要把branch01分支上的更新版本合并到主分支上的更新版本：

1、进入主分支

```c
$ git checkout master
```

2、开始合并（会出现冲突）

```c
$ git merge branch1                                         //合并命令
Auto-merging conlig.txt
CONFLICT (content): Merge conflict in conlig.txt            //显示发生冲突
Automatic merge failed; fix conflicts and then commit the result.

SmileDu@DESKTOP-EDDP6FB MINGW64 /f/桌面/GitResp (master|MERGING) //此时处在合并状态中

//什么时候会出现冲突？在同一个文件的同一个位置修改
```

3、解决冲突

先直接打开冲突文本留下想要的内容，然后将其从工作区添加到暂存区，然后commit到本地库

```c
SmileDu@DESKTOP-EDDP6FB MINGW64 /f/桌面/GitResp (master|MERGING)   //原本是在冲突状态
$ git commit -m "解决了冲突问题"                                //提交时不要加文件名字
[master 0f4c006] 解决了冲突问题  

SmileDu@DESKTOP-EDDP6FB MINGW64 /f/桌面/GitResp (master) //冲突解决之后就会回到主分支
```

# GitHub注册

www.github.com/       (最好使用除了163邮箱以外的邮箱)

## 实现本地库与远程库的交互

1、建立本地库 （之前的讲解）

```c
mkdir GitLocalResp                              //创建一个文件夹
cd GitLocalResp                                 //进入该文件夹
git init                                        //初始化使之成为本地仓库
touch Demo.txt                                  //创建一个文件(工作区)
git add Demo.txt                                //将文件由工作区添加到暂存区
git commit -m "提交第一个测试文件Demo" Demo.txt   //将文件由暂存区提交到本地仓库
```

2、创建GitHub远程库

<img src="C:\Users\SmileDu\AppData\Roaming\Typora\typora-user-images\image-20201127131942824.png" alt="image-20201127131942824" style="zoom:50%;" />

3、将本地库的东西推到远程库

首先得知道远程库的地址：

<img src="C:\Users\SmileDu\AppData\Roaming\Typora\typora-user-images\image-20201127132232552.png" alt="image-20201127132232552" style="zoom:67%;" />

![image-20201127132413928](C:\Users\SmileDu\AppData\Roaming\Typora\typora-user-images\image-20201127132413928.png)

```java
https://github.com/SmileDzh/GitRemoteResp.git
```

但是远程库地址比较长。Git本地可以将地址保存，通过别名

```c
//给远程库的地址取一个别名叫做urlAddress(随便取)
$ git remote add urlAddress https://github.com/SmileDzh/GitRemoteResp.git

SmileDu@DESKTOP-EDDP6FB MINGW64 /f/GitHub/GitLocalResp (master)
$ git remote -v                                             //查看别名
urlAddress      https://github.com/SmileDzh/GitRemoteResp.git (fetch)
urlAddress      https://github.com/SmileDzh/GitRemoteResp.git (push)
```

开始推送：

```c
$ git push urlAddress master      //选择将master分支的内容推送到urlAddress   
```

![image-20201127133534506](C:\Users\SmileDu\AppData\Roaming\Typora\typora-user-images\image-20201127133534506.png)

远程仓库就出现了本地库master分支的文件

4、克隆   （clone）

其他职员从远程库克隆刚刚项目经理从本地库推送到远程库的文件到自己本地

```c
$ git clone https://github.com/SmileDzh/GitRemoteResp.git   //后面的地址下图得到
```

<img src="C:\Users\SmileDu\AppData\Roaming\Typora\typora-user-images\image-20201127143453721.png" alt="image-20201127143453721" style="zoom: 80%;" />

克隆操作可以帮我们完成：1）初始化本地库

​                                            2）将远程库的内容完整的克隆到本地

​                                            3）替我们创建远程库的别名

5、邀请加入团队

当其他职员把远程库克隆下来的文件进行更新之后，想要再次提交到项目经理创建的远程库中，则必须由项目经理把这个职员加入团队，然后这个职员接受邀请之后，才能成功完成提交。

6、拉取操作

pull操作，当其他成员更新了远程库的内容，项目经理需要把远程库的东西拉取到自己本地库的时候，相当于fetch操作和merge操作的合并

1）首先进入项目经理的本地库文件夹

2）把远程库的内容抓取到本地库

```c
$ git fetch urlAddress master
//此时只是抓取到本地库，并没有更新到工作区
```

3）合并到工作区之前可以先查看抓取到本地库的内容是什么，将分支切换到远程库的master分支

```c
$ git checkout urlAddress/master             //切换到远程库主分支
$ ll                                         //查看
```

4）内容正确之后，就可以进行合并

```c
$ git checkout master                        //把分支切换回来
$ git merge urlAddress/master                //进行合并
```

其实也可以直接利用git pull urlAddress master来完成     

