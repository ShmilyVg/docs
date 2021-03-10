#### git使用技巧

**1：建立本地仓test 并建立追踪关系，如果建立了本地仓也建立了追踪会修改追踪关系（ 建议使用）**

`git branch --set-upstream test origin/master`

**2:  建立test仓库 并建立追踪关系** 

`git branck --track test origin/develop`

**3:  修改追踪关系**

**切换到test** 

`git checkout test`

**修改追踪仓库（一定要先切换）**

`git branch --set-upstream-to  origin/master`

**4：另一个更为简洁的方式是初次push时，加入-u参数**

`git push -u origin develop`

`git push --set-upstream origin my_remote_branch_name`

**这个操作在push的同时会指定当前分支的upstream**



**查看本地分支及追踪的分支**

`git branch -vv`

**查看远程的分支**

`git branch -a`





#### git版本回退的最佳方式



**方式一：reset（不推荐）**

通过reset的方式，把head指针指向之前的某次提交，reset之后，后面的版本就找不到了

![img](https://img2018.cnblogs.com/blog/1114289/201901/1114289-20190104171721230-1366009529.png)

操作步骤如下：

1、在gitlab上找到要恢复的版本号，如：

139dcfaa558e3276b30b6b2e5cbbb9c00bbdca96 

2、在客户端执行如下命令（执行前，先将本地代码切换到对应分支）：

git reset --hard 139dcfaa558e3276b30b6b2e5cbbb9c00bbdca96 

3、强制push到对应的远程分支（如提交到develop分支）

git push -f -u origin develop

OK，现在到服务器上看到的代码就已经被还原回去了。这种操作存在一个问题，服务器上的代码虽然被还原了，但假如有多个人在使用，他们本地的版本依然是比服务器上的版本高的，所以，别人再重新提交代码的话，你撤销的操作又会被重新，你上面的操作也就白操作了。解决办法是，让别人把本地的分支先删掉，然后重新从服务器上拉取分支

 

 

**方式二：revert（推荐）**

**这种方式不会把版本往前回退，而是生成一个新的版本。所以，你只需要让别人更新一下代码就可以了，你之前操作的提交记录也会被保留下来**

**![img](https://img2018.cnblogs.com/blog/1114289/201901/1114289-20190104172250618-1810657676.png)**

 

操作步骤如下：

1、在gitlab上找到要恢复的版本号

2、git revert -n 版本号

3、git commit -m xxxx 提交

4、git push 推送到远程

