---
layout: post
title:  "指南 | Windows 下使用 GitHub Pages 搭建个人博客"
date:   2018-03-25 07:56:00
categories: tools guidance github-pages
tags:
- Guidance
---

这两天偶然看到了一篇基于 GitHub Pages 的博文，之前对 GitHub Pages 早有耳闻，遂怀着兴趣研究了一下。这篇指南使用的是探索的视角，一步步呈现从无到有的详细过程，包括研究官方指引、带逛英文文档、分析步骤原理等。  

根据[ GitHub Pages 官网](https://pages.github.com/)的介绍，

 > Websites for you and your projects.
 > Just edit, push, and your changes are live.

使用 GitHub Pages 可以在短时间内搭建出精美、个性的博客网站。只需要专注于编辑文章内容，放入预先提供好的模板，再提交给 GitHub Pages，就可以看到自己发布的网站了。

精致，简单，免费，这真是太棒了，现在就开始吧。

<h3 style="font-weight:300">目录</h3>

* 
{:toc}

## 在 GitHub 上启用 GitHub Pages

本节主要将引导阅读[ GitHub Pages 官网][githubpages-doc]下方的快速指南。本节结束时，在浏览器输入网址就能看到属于自己的、新生的基础网站了。

官方指引的第一步：

> Head over to GitHub and create a new repository named username.github.io, where username is your username (or organization name) on GitHub.

眼下，如果你不了解Git、GitHub或repository（翻译为"库"），那么你需要阅读下面的基础准备。

### 基础准备：安装Git，拥有一个GitHub 账号

理解了原理，才能更好地学习，如果你暂时对 Git 一无所知，或者想先对 GitHub Pages 的原理有大致直观的把握？我写了一篇短文，它就是为此而生的，这篇短文将很有助于你对接下来步骤的理解：[入门 \| Git 、GitHub 与 GitHub Pages 原理的生动介绍][abc-git]

首先，从[ Git 官网](https://git-scm.com/)上下载 Git 并安装，安装完成后，右键菜单会多出 Git Bash Here 这一项即可，后面将用到。

如果还没有 GitHub账号，先在[ GitHub ](https://github.com/)上注册一个，只需不到 2 分钟。

接着，如果是刚注册 GitHub 账号，还需要关联本地 Git 客户端：在 Settings-SSH and GPG keys 中关联自己本地 Git 用户的 SSH Key，具体步骤可参考[GitHub帮助文档](https://help.github.com/articles/connecting-to-github-with-ssh/)后四篇。

基础准备完成后，你就可以正常使用 Git 和 GitHub ，下一步从官方指引出发，开始愉快的 GitHub Pages 探索之旅吧。

### 新建一个远程库

首先，登录 GitHub ，在导航条加号菜单中点击 New repository 进入新建远程库的界面（库的概念见上文的简单介绍）。

页面要求填写库的基本信息，在 Repository name (就是库名，也就是受到 Git 版本控制的文件夹名)中输入 *自己的 GitHub用户名.github.io*，比如若 GitHub 账号名是*username*，那么库名就应设置成 *username.github.io*，这个命名方式是 GitHub Pages 规定好的，这个库名其实也是我们个人网站的地址。

其他选项可默认，填写完毕后点击 Create Repository 即可。

### 将远程库克隆到本地

新建远程库之后，进入刚才的库中，将库 Clone 到本地自己的一个文件夹中。

具体步骤可以是：

1. 点击导航条上的头像菜单-点击 Your profile，打开的页面中点击 Overview 旁边的 Repositories 选项卡，选择刚建的库。

2. 在库的文件列表的上方，右侧有一个绿色按钮 Clone or download，点开，复制中部的 SSH key，比如 *git@github.com:honghzh/demo2.git*。

3. 在本地新建一个文件夹用来存放个人网站，比如命名为 My_Page，进入 My_Page，右键菜单-- Git Bash Here，在打开的 Git 终端上输入并回车执行

    ```
    git clone https://github.com/username/username.github.io
    ```

    其中，*git clone* 后面的地址是自己在第2步复制的 SSH Key，不同用户的 *username* 部分是不同的。
    
    这个命令意思是将远程库地址是 *git@github.com:username/username.github.io.git* 的远程仓库同步到本地。

### 写一个基础网页，推到远程库中

最后，在 Git 终端中依次输入并执行以下五条命令即可：

```
cd username.github.io
echo "Hello World" > index.html
git add .
git commit -m "Initial commit"
git push origin master
```

对此的解释：

第一行 cd username.github.io 意思是进入名为username.github.io的文件夹。

第二行 echo "Hello World" > index.html 是在文件夹中新建一个名为 index.html 的 HTML 文件并在其中写入  Hello World ，此处只是示例，我们等下真正写博客内容的时候并不用这种方式写入。

第三行开始使用 Git 的版本控制功能，第三行 git add . 意思就是将username.github.io文件夹里的所有变动记录到暂存区（注意命令最后有一个点）。

第四行 git commit -m "Initial commit" 意思是将之前暂存的变动作为一个新的版本提交给 Git ，这个版本的描述信息是“Initial commit”。

第五行 git push origin master 意思是将最新的本地版本同步到代号为 origin 的远程库中（在`git clone`的时候，Git 已经自动将这个本地库的关联的 origin 定义成你 clone 时复制的目标远程库， master 是默认分支名，分支相当于一条独立的版本变动记录，一个库可以有多个分支），执行完毕后刷新GitHub的远程库，会发现其中的文件被改变了，即远程的版本被你更新了。

### 新网页诞生

至此，本节完成，现在在地址栏中输入 `"https://你的用户名.github.io"`，就可以看到刚刚创建的个人网页了，虽然只有最基本的 Hello World 字样，但下一节引入官方预设的主题模板后，界面将十分美观。

## 引入网站模板

本节讲述如何在 GitHub Pages 上套用官方或社区提供的精美模板，完成后，网站将变得精致美观、五脏俱全。

### 从 GitHub Pages 到 Jekyll

在 [GitHub Pages 官网][githubpages-doc]在快速指引的结尾写着这样一段话：

> **Bloging with Jekyll**
>
> Using Jekyll, you can blog using beautiful Markdown syntax, and without having to deal with any databases. [**Learn how to set up Jekyll**][jekyll-doc].

原来，使用一个叫 Jekyll 的工具，就可以省去自己搭建网站的麻烦，只需要专注于编辑内容，这正是我们想要的。

点开它附上的链接：[**Learn how to set up Jekyll**][jekyll-doc]，我们进入了 Jekyll 官方的 Quick-start 指引。这个指引开头列出了一大堆基本命令，比如：

```ruby
# Create a new Jekyll site at ./myblog
jekyll new myblog
```

注意到第一行的注释，发现这个指引是教我们如何从一个空荡荡的网站开始，从零构建出我们的网站的。

那么，**有没有人已经按这个指引构建出一个结构完整的网站，顺带设计好样式，然后发布到网上供我们直接套用呢？**

有！他们把模板放在他们 GitHub 的仓库上，甚至还写了详细的说明文档，教你怎么使用他们的模板。许多设计精美的模板被 Jekyll 官方收录在[这里][jekyllthemes]，直接套用它们（或在其基础上自己改写）是最快最有效率的方法。

### 套用 Jekyll 主题模板

现在，访问官方收录的模板 [Jekyll Themes][jekyllthemes]，选择一个你觉得好看的模板，这里选择 Prologue 主题为例：[Prologue](http://jekyllthemes.org/themes/jekyll-theme-prologue/)，点击按钮“Download”下载它的zip文件。

解压后是一个名为“jekyll-theme-prologue-master”的文件夹，进入该文件夹，把内部所有的文件复制进你之前从自己 GitHub 上克隆下来的那个文件夹(就是和你的网站的远程仓库关联的本地仓库)。若提示冲突，则选择替换。

**不确定哪个是本地的网站文件夹（仓库）？**

* 因为从 GitHub 上 clone 下来的仓库，会自动和 GitHub 的远程仓库**同名**，所以你本地的网站文件夹（仓库）的名称应该是“你的用户名.github.io”，注意不要删掉里面的“.git”文件夹。

* 一个 Git 仓库最明显的特征是：仓库里会有一个名为“.git”的隐藏文件，那是 Git 用来做版本记录的“小本子”。


### 同步到 GitHub

GitHub Pages 实质上是用它自己的服务器，将你放在 GitHub 上的某个远程仓库（repository）渲染成你的个人网站。我们刚才已经将模板放进了自己的本地仓库中，那么剩下要做的就是把它推送到自己 GitHub 的远程仓库上，我们的个人网站就会自动更新。

在网站仓库根目录下右键-> Git Bash Here 打开 Git 终端，依次输入并执行以下三条命令：

```
git add .
git commit -m "引入网站模板"
git push origin master
```

解释：这三条命令让 Git 终端把当前仓库的状态记录为一个新的版本（前两行），并且同步到 GitHub 对应的远程仓库中（第三行）。详细同第一步中的解释。

### 精致美观的网站

现在，GitHub 上的远程仓库已经改变，访问我们的个人网址 *https://你的用户名.github.io* 就可以看到更新后的个人网站了。虽然其中的文字内容还只是模板的示例内容，但这已是从无到有，再到美观的一大进步了~

## 发表第一篇文章

本节将指引阅读刚才下载的主题模板自带的文档，从而可以修改文字内容，真正完成一个自己的网站。

首先，我们回到刚才在 [Jekyll Themes][jekyllthemes] 中挑选的主题，比如 Prologue ，看到[Prologue](http://jekyllthemes.org/themes/jekyll-theme-prologue/)页面下方的 Getting Started :

>Information and special instructions on GitHub.

好的，详细的编辑指南在他们的 GitHub 里，我们点击页面上方的 Homepage 进入 [Prologue主题的 GitHub](https://github.com/chrisbobbe/jekyll-theme-prologue)。

在这个远程仓库里向下滚动，就看到了说明文档：

（所有主题的功能架构都是统一的，因此其他的主题即使没有这份文档，配置方法也和这篇几乎相同）

### Prologue - Jekyll Theme 文档

  * 开头是主题的简单介绍。（忽略）

  * **Added Features**

    这个版本增加的新特性有...（忽略）

  * **Installation**

    引入模板的方法，这里写了两种方法。我们刚刚用的是第二种。（因为刚才已经安装好了，所以这步也忽略）

  * **Build your homepage**

    **重点**：从这里开始介绍怎么把网站修改成自己的。

    1. 网站的所有基础配置（比如网站名称、结构、作者信息等等）都在 *_config.yml* 文件里，这个文件可以用文本编辑器打开（比如 [VS Code][vscode]）修改。
    ```
    # _config.yml 摘选，只需修改冒号后的内容，不需要的放空即可
    title: Your awesome title #你的网站名称
    subtitle: Your awesome subtitle #你的网站副名称
    author: Your Incredible Name #博主ID
    email: your-email@example.com # 你的邮箱
    avatar: assets/images/avatar.jpg # Logo地址
    ...
    ```
    2. 所有你需要发布的文章，只需放入 *_posts* 文件夹里，一般里面都会有几篇示例文章，文章后缀名一般是 .md 或 .markdown ，因而这个文件夹很好识别。

       在一篇文章的开头可以进行一些标题、标题配图、标签等等的独立配置，下一个标题后会告诉你怎么配置。
  * **Start blogging !**

    这里开始教你怎么新增一篇文章。

    首先，使用文本编辑器（比如 [VS Code][vscode]）在 _posts 文件夹中新建一个 markdown 文件，比如 newArticle.md 。

    * **配置文章信息**
      
      在文章的开头进行基本信息的声明，以三个短横线 \-\-\- 开头，三个短横线 \-\-\- 结尾，比如：

      ```
      ---
      title: 文章的标题
      layout: 这篇文章
      ---
      ```
      模板就会自动把这些信息渲染到网站上合适的位置，或实现所需的功能（比如按文章标签分类），具体可以查看已有的示例文章进行理解、模仿。

      可能有的主题需要你设置文章的发表日期，注意把时间写早一点，比如比现在少8小时，否则由于时区问题，可能最后看不到自己新增的这篇文章。

    * **使用 markdown 语法写作**

      这里的文章一般使用 markdown 语法写成（比如本文），文件后缀名为 *.md* 或 *.markdown*。markdown 是一种美观易学的写作方式，已经受到很多主流的笔记类 APP 支持。只需在文本基础上增加少量符号，就可以自动美化排版。如果现在不会的话，不妨在编辑文章时按需求参考[Markdown简明语法](https://www.appinn.com/markdown/)，很快就能运用自如。

      现在就可以使用 markdown 写一篇小短文了，整个 *myArticle.md* 文件的内容可以类似这样：

      ```
      ---
      title: 文章的标题
      layout: 这篇文章
      ---

      这是我的第一篇文章。

      ## 前言

      第一段，注意markdown语法要求标题的#号后面跟一个空格，否则那只会被看作普通的#号。

      第二段，注意markdown语法要求两段之间隔一整个空行。

      ## 正文标题一

      正文第一段

      ...
      ```
      写好后保存即可，注意**文件名不应该有中文**（因为文件名也会被设置成这篇文章的子网址），一般还会要求你把文件命名为 *日期-英文简要标题.md* 的格式，比如 *2018-03-25-github-pages-guidance01.md* ，和本文的地址栏比较就很好理解了。
* **Add a page**

    这里教你怎么在网站上新建一个独立页面（比如 *About* 页面），这个页面的链接默认将在导航栏上显示。

    步骤：你只需要在网站文件夹根目录下新建一个 markdown 文件，和刚才写文章一样，写入基本信息和正文内容，保存即可。文件名是这个页面的子网址，文件里设置的 *title* 会显示在导航栏上，在文件内配置 *hide:true* 可以使该页面不显示在导航栏。

**我们的主要做：**根据文档，我们重新编辑了 *_config.yml* 文件，并在 *_post* 文件夹里把原有的示例文章删除，新增了自己的文章。

### 其他文件夹的功能

因为所有主题的文件结构都是统一的，我们还可以研究一下其他的文件夹的作用。通过浏览[Jekyll官方文档][jekyll-doc]可以知道详细，直观地说，*_includes* 就是网站的基础组件，包括导航条、网站头部等等，*_layout* 就是利用基础文件组成网站各部分页面的高级组件。*_assets* 是存放网站样式、动态效果的地方，*_section* 管理网站的主页应显示的内容，*_site* 是编译好的网页文件夹，*_index.html* 就是整个网站的入口文件。

有的主题可能不全含有上述的文件夹，但是基本功能和文件结构是大同小异的。

### 发布新增的文章

现在可以向 GitHub 发布新增的文章和改好的信息了，同样上文一样，网站文件夹根目录下，右键->Git Bash Here，依次执行下面三行命令：

```
git add .
git commit -m "新增文章，修改配置"
git push origin master
```

刚才在本地仓库作的改动就被同步到了 GitHub 的对应远程仓库上。

### 完整的个人网站

现在输入网址 *https://你的用户名.github.io/*，就可以看到你刚创建的美观、个性化的完整版网站~

此后，你只需在本地 *_post* 文件夹里新增文章，再像上一步那样（只有第二条命令的 *-m* 后面的引号内信息需要每次修改）推送到 GitHub 上就可以更新自己的网站了。

**至此，你已经完全完成了基于 GitHub Pages 的个人网站搭建，Cheers~**

---

### 继续探索？

每次修改都要推送到 GitHub 上才能看到效果，感觉太麻烦了？从 Jekyll 官方文档出发，搭建环境、配置相应文件，实现 GitHub Pages 的本地实时预览功能吧 ~

**想要更优雅的操作，本地编辑、预览 GitHub Pages？** 

继续探索 [指南 \| Windows 下 GitHub Pages 本地调试][github-pages-guidance02] 吧~


[githubpages-doc]:https://pages.github.com/
[abc-git]: /abc/github-pages/git-and-github-pages-abc
[jekyll-doc]:https://jekyllrb.com/docs/quickstart/
[jekyllthemes]:http://jekyllthemes.org/
[vscode]:https://code.visualstudio.com/
[github-pages-guidance02]:/guidance/github-pages/github-pages-guidance02