---
title: 个人Markdown Web  编辑器产品设计&部署
date: 2016-10-01 20:07:53
tags:
 - Markdown
 - StackEdit
 - Hexo
 - 同步
layout: draft
---

最近迷上了Markdown，希望积累些文字。同时，Sublime的Markdown插件已经能满足我对写作体验的追求，作为一个WebApp的狂热爱好者，决定使用StackEdit作为我的Markdown终端。另外，因为已经利用GitHub Page和Hexo建了一个Blog，必须兼容历史数据的同时具备一定的健壮性，起码，单独使用Hexo和StackEdit时，都要符合产品的使用预期。

<!-- more -->

[#为什么使用StackEdit作为我的Markdown终端？-Markdown Web  编辑器产品调研](MarkdownWeb编辑器产品调研)

# 我的场景

 - Web端Markdown撰写 - StackEdit
 - 客户端Markdown撰写 - Sublime
 - 已经利用Hexo撰写过文章并发布到GitHub Pages，GitHub Pages只负责展现
 
# 其他背景

## Hexo项目逻辑

Hexo会根据`<%hexo-root-directory>/source/_posts`下的`*.md`文件每次生成（`hexo generate`）静态HTML文件保存于`<%hexo-root-directory>/.deply_git`文件夹，每次部署（`hexo deploy`），把`.deply_git`文件夹中的HTML文件同步到GitHub。利用[GitHub Pages](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/)设置，自动从master分支build成网页。

（.md是Markdown文件后缀）


```sequence
/source/_posts->.deploy_git:hexo generate
.deploy_git->GitHub repo:hexo deploy
GitHub repo->Hexo pages:GitHub Pages
```
[#reference-hexojs/hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)
[#reference-deployment](https://hexo.io/zh-cn/docs/deployment.html)

#  构思

基于以上场景，简单的概念模型如下：


```sequence
StackEdit -> Sublime : Synchronization
Sublime -> StackEdit : Synchronization
Sublime -> GitHub pages: publish
```


我需要考虑的点包括：

 - 对所有终端，“发布”后应该保持同步
 - **撰写**包括**增 、删 、改 、查** 操作
 - 两个终端都能互相**撰写**对方创建的md
 - 两个终端应该有一套同步&冲突处理机制
 - 三个终端支持相同的Markdown语法

## 目前的解决方案

**涉及的开源项目：GitHub/GitHub Pages＋Hexo＋StackEdit**

 - StackEdit充当Web端Markdown编辑器
 - 关联StackEdit到Github，StackEdit固定发布到repo `Markdown`
 - Github repo `Markdown`与本地文件夹`<%hexo-root-directory>/source/_posts>`同步
 - 关联Hexo到GitHub repo`NoraGithub.github.io`，利用[Hexo项目逻辑](#Hexo项目逻辑)中，`<%hexo-root-directory>/source/_posts>` 和 `<%hexo-root-directory>/.deply_git` 的映射关系，发布到GitHub Pages

```sequence
StackEdit->`Markdown` Repo:git push --force
```
```sequence
`Markdown` Repo->/source/_posts:Git Synchronization
/source/_posts->`Markdown` Repo:Git Synchronization
/source/_posts->GitHub Pages: hexo generate -deploy
```


 
## 冲突处理机制 

- 由上图可见，和概念模型的区别是，StackEdit没办法获取（`git pull`/`git fetch`）到本地Sublime编写过的md文件，甚至无法知道产生过冲突，只能 `git push --force`。
- 为了防止冲突，应该尽量StackEdit撰写文章，然后单向发布文章。
- 实在必须本地Sublime编写，先`git pull` ，解决冲突后，在online的情况下，  CV（复制+黏贴）回到StackEdit，进入单向发布文章流程。


#Advanced
git module[git工具－子模块](https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)
 

## StackEdit

## git


what "github update" do

windows 下奇怪路径导致不能git pull


# 个性化方案

##[couchdb](http://couchdb.apache.org/)
[apache couchdb-wikipedia](https://zh.wikipedia.org/wiki/CouchDB)
[couched github commit](https://www.google.com.hk/search?newwindow=1&safe=strict&q=couchdb+github+commit&oq=couchdb+github+commit&gs_l=serp.3...5756.6895.0.7150.7.7.0.0.0.0.159.671.3j3.6.0....0...1c.1.64.serp..1.4.456...0i30k1j0i8i30k1j0i5i30k1j30i10k1.G7oZjLggN_Y)
[Installing on OSX](http://wiki.apache.org/couchdb/Installing_on_OSX)
[Apache CouchDB INSTALL.Unix](https://github.com/apache/couchdb/blob/master/INSTALL.Unix.md)
[couchdb setup](https://github.com/benweet/stackedit/blob/master/doc/couchdb-setup.md)
[couchdb的安装时使用](http://www.jdon.com/repository/couchdb.html)
[couchdb  stackedit 教程](https://www.google.com.hk/search?newwindow=1&safe=strict&q=couchdb++stackedit+%E6%95%99%E7%A8%8B&oq=couchdb++stackedit+%E6%95%99%E7%A8%8B&gs_l=serp.3...1737704.1740833.0.1740989.21.12.9.0.0.0.207.867.4j3j1.8.0....0...1c.1.64.serp..5.2.209...0i8i30k1.J1f7Jf6RHzQ)



# 如果可以的话

理想状态下，Editor和Publisher应该在部署在同一个Server，作为同一个Website。

## 理想模型

### 未发布

```sequence
StackEditor->Browser:Cache()
Browser->StackEdit Preview:present
StackEditor-->StackEdit Preview:WYSIWYG
```
WYSIWYG：What You See Is What You Get，所见即所得

### 发布

```sequence
StackEdit->.md:publish
.md->Hexo:present
.md-->Local:Synchronization

```
一旦冲突可以出线命令行界面，用于merge或rebase。
由于本文所有项目都是开源的，相信终有一天能实现。

 - [x] GitHub Pages只负责展现，不能撰写
 - [x] 对StackEdit/Sublime，存在“发布”/“未发布”状态，“发布”后应该尽量同步，“未发布”状态相互独立即可
 - []所谓“同步”，完全按照Git Flow管理（如果有协作，可能GitHub Flow更好）
 - []三个终端支持相同且完整的Markdown语法，这套语法应该涵盖UML、LaTex公式& MathJax在内的分析师、PM常用表现工具

```sequence
Local-->GitHub:git push
GitHub-->StackEdit:git push
Local-->Hexo:hexo g -d
```