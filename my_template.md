---
layout: default
---

Template for my blog
===========================================

>转载请标明出处：  
>https://seektech.github.io/ [**Miao LI (seektech)**](https://seektech.github.io)


完全使用命令行操作`GitHub`,学习笔记，持续更新

### [](#header-2)一、将新创建的本地代码上传到github上

**step 1:** 建立本地版本仓库，cd到要进行版本管理的project目录下，假设project名称为MiaoLI，执行完该命令后，当前目录下多了一个隐藏的.git文件夹

```js
cd /MiaoLI
git init
```
**step 2:** 将MiaoLI目录下的文件或者文件夹添加到仓库

```js
git add .
```

**step 3:** 将add的操作commit到本地仓库

```js
git commit -m "注释"
```

**step 4:** 在`GitHub`上建立仓库，建议与本地project同名，也可异名，然后执行如下命令是

```js
git remote add origin ...
```
**step 5:** 将本地仓库的操作提交到`GitHub`上，提交之前pull一下,执行完如下命令不报错则上

```js
git pull origin master
git push -u origin master
```


### [](#header-2)二、从github上clone代码，添加文件或者修改文件后提交

**step 1:** 将代码clone到本地，clone成功后会在本地生成一个与所clone的`GitHub`仓库同名的文件夹，

```js
git clone ...
```

**step 2:** 执行add命令之前可以使用git status命令查看仓库当前的状态（添加和修改了哪些文件）

```js
git status
git diff
git add .
```

**step 3:** 然后执行commit命令将更改提交的本地仓库，执行commit之前可以git status查看当

```js
git status
git commit -m "注释"
git status
```

**step 4:** 将本地仓库的更改push到远程仓库

```js
git pull origin master
git push -u origin master
```
如有评论和建议，请移步[CSDN](http://blog.csdn.net/u013413471/article/)  

Back to my [HOMEPAGE](index).