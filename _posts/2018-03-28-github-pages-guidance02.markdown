---
layout: post
title:  "指南 | Windows 下 GitHub Pages 本地调试"
date:   2018-03-26 18:00:00
categories: tools guidance github-pages
tags:
- Guidance
---

本文将带你以 Jekyll 英文文档为起点，延伸探索、集齐龙珠，实现**实时预览**的 GitHub Pages 写作体验。

GitHub Pages 系列文章：

1. [杂谈 \| Git 、GitHub 与 GitHub Pages][github-pages-abc]
1. [指南 \| Windows 下使用 GitHub Pages 搭建个人博客][github-pages-guidance01]
1. [指南 \| Windows 下 GitHub Pages 本地调试][github-pages-guidance02]

<h3 style="font-weight:300">目录</h3>

* 
{:toc}

# 前言

实际上，用 GitHub Pages 写文章，我们只需要下载模板、修改模板、推送到 GitHub ，三步就能实现（从[上一篇][github-pages-guidance01]容易体会）。然而，每次修改模板后都需要推送到 GitHub 上才能看到自己编辑的效果。

有没有方法可以在本地浏览器上直接看到自己编辑的效果，只要有微小改动，保存后轻点刷新，就能看到最新的预览呢？

[上一篇][github-pages-guidance01]我们跳过了 Jekyll 大部分文档，直接下载主题模板，为了解决这个问题，我们不得不重新打开 [Jekyll 文档][jekyll-doc]，研究 Jekyll 的些许运行原理了。

# 在 Jekyll 文档

打开 [Jekyll 英文文档][jekyll-doc]，Quick-start guide 第一行写着（注意粗体即可）：

**If** you already have a full [Ruby][ruby] development environment with all headers and RubyGems installed (see Jekyll’s requirements), **you can** create a new Jekyll site by doing the following:...

If...you can...，原来，若想要在自己本机上运行一个使用了 Jekyll 的网站，我们只需达成 If 中的两个**前提条件**：

1. 你的电脑要能看得懂 Ruby 语言
2. 还需要名为 RubyGems 的工具

很棒，只需要两个条件，接下来我们一一满足它们就好了。

# 第一个条件：让你的电脑看懂 Ruby 语言

我们的电脑在被主人买回家的时候，虽然已经自学了很多语言，但是委实没有学过 Ruby 。

也就是说，当它的主人（也就是你）用 Ruby 语言跟它说话时，它完全听不懂你想让它做什么。

**显然，它需要一个翻译。**

这位 Ruby 语翻译先生，在计算机世界就叫做 *Ruby 解释器* ，刚刚 [Jekyll 英文文档][jekyll-doc]很贴心地在 "Ruby" 字样上设置了超链接，点击 [Ruby][ruby] ，我们进入了 *Ruby 解释器* 官网的英文下载页面。

页面中，看到小标题 Ways of Installing Ruby：

>**Ways of Installing Ruby**
>
>We have several tools on each major platform to install Ruby:
>
> * On Linux/UNIX, ...
> * On OS X machines, ...
> * **On Windows machines, you can use [RubyInstaller][RubyInstaller].**

根据不同的操作系统，有不同的方法安装解释器，在 Windows 下推荐使用 RubyInstaller，于是，我们点进 [RubyInstaller 官网][RubyInstaller]，点击大红色的 Download ，进入下载页面选择版本。

我们要选择 GitHub Pages 依赖的那个 ruby 版本，百度"GitHub Pages dependence"找到[GitHub Pages 依赖版本说明][github-pages-depend]，我们可以知道 *Ruby 2.4.3-2* 是我们需要安装的版本。

### 安装 MSYS2 集成

目前看起来一切太平，但接下来的步骤就有不少需要注意的点，一不留神就会报错失败，现在让我们步步为营，继续安装这个 Ruby 解释器。

在安装的最后一步，RubyInstaller 会询问你：是否执行 *ridk install* 命令以安装 MSYS2 ？这是什么？捆绑软件吗？

别冲动，让我们回到 [RubyInstaller 下载页面][RubyInstaller]找线索，在官网右侧找到了 MSYS2，注意下面粗体和斜体部分：

Starting with Ruby 2.4.0 we use the **MSYS2 toolkit** as our development kit. It is **required** to *build native C/C++ extensions for Ruby* and is necessary for Ruby on Rails.

原来，MSYS2 是一个拓展工具（extensions），而且是 **required**。有了它，这位 Ruby 翻译就可以额外翻译一些 Windows 听不懂的 Linux 命令。

>**如果现在没有安装 MSYS2 的全部集成的话，最后一步安装 Jekyll 就会报错失败**：
>
>"无法识别 *make* 命令"
>
>因为*make* 是写在 Jekyll 安装代码中的 Linux 系统命令。如果没有 MSYS2 的翻译拓展，Windows 将完全不懂那条命令想要它干啥。

于是我们勾选安装 MSYS2，会弹出一个命令行界面，询问你想安装 MSYS2 的哪一部分。先输入1，安装 MSYS2 核心文件。

#### 1 : 安装 MSYS2 核心

输入1回车后，系统会提示在指定目录找不到安装包。提示信息让我们去访问 [MSYS2 的下载地址][msys2-down]，选择第一个i186版本下载，并下载到特定的文件夹中（根据提示耐心寻找，除了"user"可能指"用户"文件夹外，其余应一模一样）。

**在命令行界面程序运行的任何时候，你都可以连按两次 ctrl+C 终止任务。**


下载好后回车，若安装未继续，则退出程序后重新在命令行里输入 *ridk install* ，按 1 ，等待核心安装完成。

#### 2、3 : 安装 MYSYS2 剩余组件

核心安装完成后，还需要安装 MYSYS2 的剩余组件。

>安装 MYSYS2 的剩余组件将使我们的系统能够把等下 Jekyll 安装包中的 Linux 命令解读成它看得懂的语言，最终才能顺利安装。
> 
>MYSYS2 剩余的组件默认下载源是国外服务器，因为众所周知的原因**速度奇慢**，因此我们要先把 MYSYS2 的默认下载源改成国内的清华大学镜像源。

现在，访问[ MYSYS2 清华大学镜像][mysys2-qinghuamirror]，并打开本地 MYSYS2 的根目录，按照网页中 **pacman 的配置** 这一节修改相应三个配置文件即可。

接着，命令行中再次输入 *ridk install* ，依次输入 2 和 3 ，即可自动安装 MYSYS2 剩余组件。

### 看得懂 Ruby 语言了

在命令行中执行 *ruby -v* ，看到版本号说明 Ruby 解释器安装成功，你的电脑现在看得懂 Ruby 语言了（这对接下来的安装很关键）~

# 第二个条件：安装 RubyGems

回到 [Jekyll 文档][jekyll-doc]，第二个条件是安装 RubyGems 。于是我们进入 [RubyGems 下载页面][rubygems-down]，按照其中的指引，很简单，三步完成安装：

1. 点击下载 ZIP 文件，解压到你想安装 RubyGems 的目录
1. 进入解压后的文件夹根目录
1. 按住 Shift 并单击鼠标右键，选择"在此处打开命令窗口"，命令行执行 *ruby setup.rb* ，安装完成。

RubyGems 是一个包管理工具（类似于 Ruby 语言世界中的"软件管家"），可以安装或卸载各种用 Ruby 语言写成的工具，其中就包括强大的网页生成器 —— 我们已经很眼熟的 Jekyll 。下一步就是**使用 RubyGems 安装 Jekyll 包**，就能在本地运行 Jekyll 网页，实现本地预览了。

# 使用 RubyGems 本地安装 Jekyll 

所有前提条件已经达成，你的电脑拥有了解析 Ruby 语言的能力，又有了安装 Ruby 包的管理工具。现在只需用 RubyGems 把 Jekyll 安装到本地，就可以用本地的 Jekyll 构建实时预览网页了。

通过阅读 [Jekyll 文档-安装](https://jekyllrb.com/docs/installation/)和修改 RubyGems 默认下载源的教程，得到科学的安装步骤如下：

1. **修改 RubyGems 的默认下载源**（否则下载速度奇慢）

    在网站文件夹的根目录打开命令窗口（可以按Shift后从右键菜单打开），依次执行：

    ```
    gem sources –remove https://rubygems.org/
    gem sources -a http://gems.ruby-china.org
    gem sources -l
    ```

    对此的解释：
    第一行移除默认的国外下载源；
    第二行增加国内下载源；
    第三行查看现在的下载源，确保只有 *http://gems.ruby-china.org*
      
      
2. **安装 Jekyll 和 Bundler**

    命令行执行：
    ```
    gem install jekyll bundler
    ```

    其中 bundler 是一个依赖同步工具，可以确保互相依赖的各个工具的版本相契合，方便使用 Jekyll。

3. **本地运行你的网站**

    这一刻终于来了，现在我们在本地安装了 Jekyll，可以用它预览网站了

    在网站文件夹根目录下命令行执行：

    ```
    bundle init
    ```

    这会在根目录下生成一个名为 *gemfile* 的文件，包管理工具 **RubyGems 会读取这个文件，从而自动帮你安装（你下载的）网站模板所需要的额外插件**。

    使用文本编辑器（比如 [VS Code][vscode]）编辑 *gemfile* 文件：
    
    * 把 source 改成国内的 "http://gems.ruby-china.org"。
    
    * 在最下面增加几行，内容是 *gem "网站需要的插件名"*（在模板文档或 *_config.yml* 里有，如果模板自带了 *gemfile* 那么此处一般不需修改，只需修改 *source* ）
    
    * *git_source* 那行（若有）前面可以加 *#* 号注释掉。

    改好后保存，继续在根目录命令行执行：

    ```
    bundle update
    ```

    这会让 RubyGems 根据你的 *Gemfile* 文件安装 (**或以后的更新**) 所需依赖。

    **在网站文件夹根目录执行：**

    ```
    jekyll serve
    ```

    并在浏览器地址栏中输入本机4000端口地址：***localhost:4000***即可。

# 本地预览你的网站

**此后，每当需要本地预览时，在网站文件夹根目录执行：**

```
jekyll serve
```

并在浏览器中输入本机4000端口地址：***localhost:4000***，就可以看到自己网站的本地预览了~

只要预览程序运行，刷新浏览器窗口就可以即时更新你的预览。在命令行窗口内连续两次 ctrl+C 可以退出预览。

**Futher more...（选读）**

你也获得了发布网站的另一种姿势：使用本地 Jekyll 引擎先编译网站文件夹，再把编译好的文件同步到 GitHub 仓库，本地编译网站只需一步：

```
jekyll build
```

编译好的文件全部在根目录的 *_site* 文件夹里，将里面的所有文件拷贝到 GitHub 对应的本地网站仓库，右键 Git Bash Here 执行以下命令即可：

```
git add .
git commit -m "自己定义提交信息"
git push origin master
```

至此，你已经可以灵活运用基于 GitHub Pages 搭建的个人网站了

# 其他技巧

## markdown 进阶配置

可以用 markdown 的一个解析器 kramdown 来实现更多的 markdown 渲染功能。比如输入 `{:toc}` 自动生成目录。

需要先在 _config.yml 中配置如下：

```
markdown: kramdown
kramdown:
  toc_levels: 1..6
```

### 在 markdown 中生成目录

第一行输入一个无序列表 `* ` 或有序列表 `1. `，第二行输入 `{：toc}` 即可按第一行的格式渲染出全文目录

```
* 
{:toc}
```

在不需要纳入目录的标题下面加上 `{:no_toc}`

```
# 我不想进入目录
{:no_toc}
```

## QA

## 缺少依赖

如果报错 It looks like you don't have XXXXXX or one of its dependencies installed. 说明缺少 XXXXXX 包的依赖，需要先在 gemfile 里添加相应的依赖，再 `bundle update` 。

在 Windows 系统上还会进一步报错 No source of timezone data could be found. 解决方法：在 gemfile 文件中增加依赖 tzinfo-data，在 `bundle update` 即可。

```
# gemfile 文件中新增
gem "tzinfo"
gem "tzinfo-data"
```

或直接安装（不推荐）：`gem install tzinfo-data` 。

## 时区问题

在中国，由于时区问题，发布的文章8小时后才能看到，解决方法是在 markdown 的头信息 `date` 后面加上时区配置，比如：

```
---
date: 2018-08-10 08:00:00 +0900
---
```

## 在 markdown 代码块中展示模板语法 `{% raw %}{% %}{% endraw %}` 

[参考的是这篇博客的方法](http://markzhang.cn/blog/2013/11/07/embed-liquid-codes-in-blog/)

方法一：将代码放到 _includes 的一个文件中，使用 {% raw %}`{% include somefile.html %}`{% endraw %} 引入。

方法二：使用 Jekyll 支持的 raw 、 endraw 标签将要展示的模板语法代码包起来即可

```
markdown 源码：
{{ "{% raw " }}%} {%raw%}{% include somefile.html %} {%endraw%}{{ "{% endraw " }}%}
多行代码的情况下，将 {{ "{% raw " }}%} 和 {{ "{% endraw " }}%} 分别放在代码块第一行开头和最后一行末尾即可
```

要想引用 `{{ "{% raw " }}%}{{ "{% endraw " }}%}` 自身，需要这样：

```
markdown 源码：
{%raw%}{{ "{% raw " }}%}{{ "{% endraw " }}%}{%endraw%}
```

## Jekyll 进阶指南

Jekyll 是一个很好的静态博客引擎，[另一篇文章：Jekyll 静态博客引擎]({% post_url 2018-08-10-jekyll %})有助于理解整个 Jekyll 的运行原理，然后就可以进一步地制作自己的主题了。

## Reference

* [Jekyll 文档](https://jekyllrb.com/docs/structure/)
* [Ruby 文档](https://www.ruby-lang.org/en/documentation/)
* [MSYS2 官网](http://www.msys2.org/)
* [RubyGems 官网](https://rubygems.org/pages/download)
* [GitHub Pages 依赖版本说明](https://pages.github.com/versions/)
* [Jekyll 主题模板收录](http://jekyllthemes.org/)

[github-pages-guidance01]:/guidance/github-pages/github-pages-guidance01
[github-pages-guidance02]:/guidance/github-pages/github-pages-guidance02
[github-pages-abc]:/abc/github-pages/git-and-github-pages-abc
[jekyll-doc]:https://jekyllrb.com/docs/quickstart/
[ruby]:https://www.ruby-lang.org/en/downloads/
[RubyInstaller]:https://rubyinstaller.org/
[github-pages-depend]:https://pages.github.com/versions/
[msys2-down]:http://www.msys2.org/
[mysys2-qinghuamirror]:https://mirrors.tuna.tsinghua.edu.cn/help/msys2/
[rubygems-down]:https://rubygems.org/pages/download
[vscode]:https://code.visualstudio.com/