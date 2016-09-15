---
title: Hexo＋Github pages搭建第一个博客
date: 2016-09-15 18:31:09
tags: 
  - Hexo
  - github
---
  好久之前就想自己搭一个博客来玩玩了，可惜太懒一直没去弄。直到最近看到了Hexo使用hexo＋github pages搭博客不仅简单，而且还免费。我这么好(kou)学(men)的好孩子怎么能不插一脚呢！果断弄起来呀！
## 一、git
### 1.1 下载安装git 
git的安装可以通过下面两种方法：
1.去git的官网中下载{% link Git for Mac  https://git-scm.com/download/mac %}
2.去github的官网下载{% link GitHub Desktop https://desktop.github.com %}
下载完成后按照提示完成安装。
### 1.2 配置git
完成安装后需要进行一些配置，才能正常的使用git。
#### 配置SSH keys
首先检查SSH key
```bash
$ cd ~/.ssh
```
如果提示：No such file or directory说明第一次使用git，然后执行以下步骤生成新的SSH Key
```bash
$ ssh-keygen -t rsa -C "email@youremail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>
Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>
```
可以直接按3次回车，也可设置密码。这个密码会在提交项目时使用，如果为空（直接按3次回车则未空）的话提交项目时不用输入。
#### 向ssh-agent添加key
首先保证ssh-agent可运行
```bash
$ ssh-agent -s
```
添加SSH key
```bash
$ ssh-add ~/.ssh/id_rsa
```
#### 添加SSH key到GitHub
在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置
1.拷贝key
```bash
$ clip < ~/.ssh/id_rsa.pub
```
2.在GitHub右上方点击头像，选择“Settings”
{% image /images/settings.png %}
3.在左侧导航中选择“SSH and GPG keys”,然后在右上角点击“New SSH key”
{% image /images/newssh.png %}
4.将第1步中拷贝出来的key粘贴到“key”中，然后点击“add ssh key”。
{% image /images/addssh.png %}
#### 设置用户名和邮箱地址
``` bash
$ git config --global user.name "username"
$ git config --global user.email "username@example.com"
```
### 1.3 测试链接
```bash
$ ssh -T git@github.com
```
如果出现以下信息
```bash
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```
就键入：yes。之后就能看到如下信息：
```bash
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
```
## 二、创建github仓库
### 2.1 在github中创建blog的仓库
点击github个人主页的右上角的"new repository"创建新的仓库
{% image /images/newrepo.png %}
在创建页面中只需要填写repository name即可，repository name的格式必须是name.github.io 。这样github pages 才能识别出来。
### 2.2 克隆本地仓库
在github上创建好blog的仓库后可先行科隆到本地，这一步不做也没关系，因为后面的部分操作可能导致此步报废。
```bash
$ git clone git@github.com:username/name.github.io.git
```

## 三、Hexo
Hexo的安装过程可以在{% link Hexo https://hexo.io/zh-cn/docs/index.html %}的官网中找到。
### 3.1 Hexo安装前准备
安装Hexo之前需要确保机器上有npm和Nodejs。高版本的Nodejs中包含了npm，所以直接安装Nodejs即可。
Nodejs可直接在{% link Nodejs官网 https://nodejs.org/en/ %}下载安装包，根据提示完成安装即可。

### 3.2 Hexo安装
hexo是需要在用户拥有完全控制权的目录下进行安装的，可直接在2.2中克隆出来的仓库路径下进行安装。
``` bash
$ sudo npm install -g hexo
$ hexo init 
$ npm install
$ npm install hexo-deployer-git --save
```
查看克隆的目录下是否还存在.git
{% image /images/ifgit.png %}
若不存在则执行以下命令
```bash
$ git init
$ git remote add origin git@github.com:username/name.github.io.git
```
然后就可以将hexo项目提交到github中管理
```bash 
$ git checkout --orphan hexo
$ git add .
$ git commit -m "first post"
$ git push origin hexo
```
### 3.3 配置发布
在blog的仓库目录下现在会有_config.yml，编辑此文件。
默认生成的_config.yml中有这一段
```bash
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type:
```
修改后如下
```bash
deploy:
  type: git
  repo: 对应仓库的SSH地址（可以在GitHub对应的仓库中复制）
  branch: 分支（我使用master分支存放发布后的静态文件，用hexo分支存hexo项目文件）
```
PS：配置文件中“:”后面必须有一个空格。
修改成功后执行下列指令进行部署
```bash
$ hexo g
$ hexo d
```
然后访问namme.github.io就可以看到blog了。

## 四、参考链接
1. {% link GitHub-Pages-Hexo搭建博客 http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo搭建博客/ %}
2. {% link NexT主题 http://theme-next.iissnan.com/getting-started.html %}
3. {% link 如何搭建一个独立博客——简明Github Pages与Hexo教程 http://blog.csdn.net/poem_of_sunshine/article/details/29369785 %}
