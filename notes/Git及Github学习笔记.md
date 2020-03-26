# Git及Github学习笔记

> 学习自[Git使用教程](https://www.imooc.com/article/20411)

## 一、Git是什么
分布式版本控制系统

![图片描述](../image/git架构图.jpg)

Remote: 远程仓库

Repository: 本地仓库

Index: 暂存区

Workspace: 工作区

## 二、SVN与Git区别

SVN是集中式的，而Git是分布式的

SVN更安全，便于管理，版本更可控，但是数据库压力大，必须连接服务器

## 三、windows操作系统安装Git

git官网: https://git-scm.com/

官网下载默认安装即可
开始菜单找到 Git --> Git Bash
填写用户名及邮箱

	git config --global user.name "William66886"
	git config --global user.email "william66886@sina.cn"

**--global 参数 表示这台电脑所有Git仓库都会用这个配置**

## 四、具体使用

### 一、创建本地版本库

```` git
mkdir testgit
cd testgit
git init
````

### 二、将文件添加到版本库

#### 1、将文件添加到暂存区

````git
git add readme.txt
````

#### 2、将文件从暂存区提交到版本库

````git
git commit -m "readme.txt提交"
````

#### 3、查看状态

````git
git status
````

#### 4、查看修改内容

````git
git diff readme.txt
````

### 三、版本回退

#### 1、查看日志

````git
git log
git log –pretty=oneline 只显示版本号及注释
````

查看详细操作日志

````
git reflog
````

* git log 只显示已提交的版本信息

* git reflog 可以查看所有操作信息包括commit和reset

#### 3、版本回退

回退到上一版本

````
git reset --hard HEAD^
````

回退到上上版本

````
git reset --hard HEAD^^
````

回退到上100版本

````
git reset --hard HEAD~100
````

回退到指定版本

````
git reset --hard 版本号
````

#### 4、撤销修改和删除文件

撤销修改

````
git checkout -- readme.txt
````

撤销与回退区别

* 撤销会把本地当前修改但并没有提交到版本库或暂存区的内容撤销到与版本库或暂存区一致状态
* 回退会把当前版本库版本回退到上一版本

删除文件

````
 rm readme.txt
 git commit -m "删除readme.txt"
````



## 五、远程仓库Github

### 一、设置SSH  key

由于本地git仓库和github仓库是通过SSH加密的，所以需要设置SSH key

#### 1、创建SSH key 

先查看用户目录下(C:\Users\William\.ssh)是否有id_rsa id_rsa.pub两个文件

id_rsa：私钥

id_rsa.pub：公钥

如果没有，在命令行下运行

````
ssh-keygen -t rsa –C “youremail@example.com”
````

ssh-keygen参考说明:(https://www.iteye.com/blog/killer-jok-1853451)非必要知识

#### 2、github配置SSH key公密

Settings --> SSH and GPG keys --> New SSH key --> 填写Title --> 复制id_rsa.pub到key

### 二、添加远程仓库

点击右上角+号 --> create a new repo
![图片描述](../image/新建仓库设置.png)

常用开源许可证关系图
![](../image/开源许可证关系.png)

### 三、本地仓库关联远程仓库

````
git remote add origin https://github.com/tugenhua0707/testgit.git
````

上传本地代码

````
git push -u origin master
````



## 六、创建与合并分支

git checkout -b dev：创建并切换分支

git branch：查看当前分支





