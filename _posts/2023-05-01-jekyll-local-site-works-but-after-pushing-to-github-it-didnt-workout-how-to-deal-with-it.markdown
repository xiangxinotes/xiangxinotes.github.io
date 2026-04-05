---
layout: post
title: 本地 Jekyll 构建一切顺利，GitHub Pages 却部署失败？这问题好解决！
date: 2023-05-01 19:38:35 +0800
description: GitHub Pages 个人网站搭建完成，本地 Jekyll 构建一切顺利，但 push 到 GitHub 后，却收到了 pages build and deployment workflow run failed 邮件，GitHub Pages 也没更新？只花7分钟，本文就能助你解决难题！本文包含报错的根本原因和4个解决问题的方案及步骤。
image: jekyll-version-workflow-failed.webp
tags: github-pages jekyll tech
---

GitHub Pages 个人网站搭建完成，本地 Jekyll 构建一切顺利，但 push 到 GitHub 后，却收到了 pages build and deployment workflow run **failed** 邮件，GitHub Pages 也没更新？向西花了两天时间解决，而你只要花7分钟！

## 根本原因

你本地的 jekyll 版本和 GitHub 用的 jekyll 版本不匹配！【2023年5月】 GitHub 用的 jekyll 版本是 v3.9.3

想知道你的本地 jekyll 版本？在 Terminal 输入以下命令：
```
jekyll -s
```

通常情况下，你的本地 jekyll 版本会比 GitHub 版本高。所以如果你用了 v3.9.3 之后的才出现的语法就会出现本地构建成功，而GitHub 构建失败的情况。

## 解决问题

本地 `jekyll serve` 构建成功后，你可以从以下四种方案任选一个：
1. 只 `push` _site/ 文件夹到 `main` branch 上。相当于 GitHub 存放全静态网站文件。
2. `push` 所有文件 (包括 _site/ 文件夹) 到 `main` branch 上。GitHub Pages 选择 build from main branch, _site/ folder.
3. `push` 程序文件到 (不包括 _site/ 文件夹) `main` branch 上。新建 `gh-pages` branch，只 `push` _site/ 文件夹到 `gh-pages` branch 上。GitHub Pages 选择 build from gh-pages branch.
4. `push` 程序文件到 (不包括 _site/ 文件夹) `main` branch 上。新建 `gh-pages` branch。使用 GitHub Actions，使其在每次 `push` 之后自动跑 Workflow，自动 `push` 静态网站文件到 `gh-pages` branch 上。GitHub Pages 选择 build from gh-pages branch.

上文及下文出现的 `main` branch 是我存放程序文件的分支，请在执行步骤时，把 `main` 换成你使用的分支名。

我选择了方案4. 因为方案4有以下优点:
- 让我省了手动删除、上传 _site/ 文件夹的时间，
- 让我自由使用 GitHub 不支持的插件。也就是说[tag 目录 404 问题]({{ page.previous.url }})解决起来更方便了。

你也看上了方案4，却不知怎么操作？下面我以本网站的实现为例，带你走完全程！

### 一、在 Terminal 输入以下指令：
1. `jekyll serve`
2. `git push -u origin main`

请通过第一条指令确保你的网站在本地构建是成功的！

再通过第二条指令把程序文件 (不包括 _site/ 文件夹) `push` 到 `main` branch 上。

### 二、新建 `gh-pages` branch

1. 打开[GitHub](https://www.github.com)，进入存放代码的仓库。
2. 新建 `gh-pages` branch

这里新建的 `gh-pages` branch 将会存放自动生成的静态网站文件。

⚠️ 注意：如果你已经有 `gh-pages` branch 了，可以跳过这一步或创建另一个名字的 branch。

### 三、创建 GitHub Actions Workflow

1. GitHub 切换到 Actions 标签，左侧面板，点击 New workflow，在 Choose a workflow 页面选择 set up a workflow yourself. 会自动创建 .github/workflows/main.yml 文件。
![点击 New workflow 创建一个 GitHub workflow]({{ site.baseurl }}/images/jekyll-version-new-workflow.webp)
*GitHub 切换到 Actions 标签，左侧面板，点击 New workflow*
1. 按照 [Deploy Jekyll Site](https://github.com/marketplace/actions/deploy-jekyll-site#example-usage) 里的说明操作。2023年5月的你需要做两件事：
  - 在 `_config.yml` 中加一行：`destination: ./build`
  - 把说明提供的内容粘贴到你的 .github/workflows/main.yml 中。（注意把 branches 数组里的 branch name 改成你使用的分支名。
  ![我的 .github/workflows/main.yml 文件内容截图]({{ site.baseurl }}/images/jekyll-version-main-yml.webp)
  *.github/workflows/main.yml 内容*
1. 右侧点击 Commit changes... 按钮。填写 Commit message，点击 Commit changes。
![填写 Commit message，点击 Commit changes]({{ site.baseurl }}/images/jekyll-version-commit-changes.webp)
*提交更改*

这里创建的 workflow 会在 `main` branch 发生 `push` 时自动将静态网站文件 `push` 到 `gh-pages` branch 上。

提交后，此 workflow 会自动 run。因为你提交此文件的行为就是发生在 `main` branch 上的一次 `push`。

如果 workflow 还是失败，点击邮件中的 View Workflow Run 按钮，会跳转到 GitHub 上，在这里你可以检查到底是哪里出了问题。如果是以下提示：
```
remote: Permission to (...) denied to github-actions[bot].
```
那么你需要执行以下步骤：
1. GitHub 切换到 Settings 标签，左侧面板选择 Actions，选择 General。右侧找到 Read and write permissions，勾选此选项。
2. GitHub 切换到 Actions 标签，找到失败的那个 workflow，右上角 Re-run all jobs 即可。

### 四、GitHub Pages 选择 build from gh-pages branch.

GitHub 切换到 Settings 标签，左侧面板选择 Pages。右侧 Build and deployment，Source 选择 Deploy from a branch，Branch 选择 gh-pages, /(root)。稍等1～2分钟，点击 Visite site 来打开它。
![GitHub Pages 设置 build from gh-pages branch]({{ site.baseurl }}/images/jekyll-version-build-from-gh-pages.webp)

这里是为了让 GitHub Pages 用我们的静态网站文件来构建网页。

问题解决！🎉🎉🎉

## 参考资料
- [GitHub Actions - Deploy Jekyll Site](https://github.com/marketplace/actions/deploy-jekyll-site)
- [remote: Permission to git denied to github-actions[bot]](https://github.com/ad-m/github-push-action/issues/96)
- [Support for Jekyll 4.0](https://github.com/github/pages-gem/issues/651)
- [Easily Deploy your jekyll blog using Github](https://sujaykundu.com/blog/introducing-devlopr-easily-deploy-your-jekyll-blog-using-github-pages-and-github-actions/)

---

问题解决了吗？开心吗？有成就感吗？如果有帮到你，哪怕一丢丢，就把这篇文章分享给其他遭遇困难的朋友，一起升级打怪吧！～ ❤️

下面是此文章面世的原因，如果你感兴趣，继续往下看👇

## 背景 Context

此网站 hosted on [GitHub](https://www.github.com), powered by [Jekyll](https://github.com/jekyll/jekyll) and based on [Zolan theme](http://jekyllthemes.org/themes/zolan/).

从[我的网站]({{ site.url }})问世后，我一直致力于更新她，让她从主题状态变成我更中意的状态、让她展现更多功能、让她更有帮助。

最近添加的功能是：
- 在文章概览区域显示阅读时长
- 添加中国大陆地区也可使用的评论区

![在文章概览区域显示阅读时长]({{ site.baseurl }}/images/jekyll-version-minutes-read.webp)
*在文章概览区域显示阅读时长*

本地构建成功后，我就把这期间的 commit `push` 到 GitHub 中，结果出现了构建失败的邮件。

![GitHub workflow run failed email with a 'View workflow run' 按钮]({{ site.baseurl }}/images/jekyll-version-workflow-failed.webp)
*GitHub 构建失败的邮件内容*

我遇到的问题是：

```
github-pages 228 | Error:  Liquid error (/github/workspace/_includes/article-content.html line 42): wrong number of arguments (given 2, expected 1) included
```

可以看出是 _includes/article-content.html 的42行有问题。 _includes/article-content.html 的42行如下：
```
{% raw %}{% assign words = post.content | strip_html | number_of_words: "cjk" %}{% endraw %}
```

看起来没什么问题。这一行创建了一个新变量 `words`，拿到文章正文 (`post.content`)，去除html标签 (`strip_html`)，统计中英文字符数 (`number_of_words: "cjk"`)。

这里出现错误的原因是：`number_of_words: "cjk"` 是 jekyll v4.1.0 才开始支持的 Filter，而 GitHub 默认使用 jekyll v3.9.3 部署 site。jekyll 版本不匹配，所以本地构建成功，而 GitHub 失败。

而我的本地 jekyll 版本是 v4.3.2。so...
