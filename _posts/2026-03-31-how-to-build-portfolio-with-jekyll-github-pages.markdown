---
layout: post
title: 计算机学生如何做作品集？用 Jekyll 和 GitHub Pages 零成本搭建
date: 2026-03-31 22:43:00 +0800
description: 在为昂贵的服务器费用发愁？作为计算机专业学生，利用 Jekyll 和 GitHub Pages 可以零成本搭建专业级作品集。本文精选 jekyllthemes.io 等主题网站，手把手教你如何将课程作业转化为求职利器。
image: pexels-greta-hoffman-7859575.jpg
tags: college-student personal-growth github-pages
keywords: "计算机专业建议,GitHub使用,GitHub Pages,作品集搭建,计算机学生求职,程序员面试,计算机专业工作,个人作品集,大学生求职,应届生求职,应届生实习,计算机求职,计算机实习,搭建作品集,如何成为程序员,成绩差的应届生"
---

在之前的文章[《给计算机专业学生的5条建议》](/2026/03/20/advices-for-computer-science-students/?utm_source=blog&utm_medium=post)中，我们聊到了一个观点：**在求职市场上，一份个人作品集比枯燥的成绩单更有说服力。**

如果你认同这个观点，但是觉得搭建个人作品集太难、要花太长时间了，你的感觉是有道理的，但事情也可以很简单。

利用 GitHub Pages 配合 **Jekyll 主题**，你可以在**零成本**的前提下，用**最快的时间**搭建出一个专业级的作品集。

## Jekyll 是什么？

你并不需要花很多时间摸透 Jekyll，因为我们的目标是打造作品集，而不是开发一个复杂的网站。

你只需要知道，Jekyll 允许你用 Markdown 编写内容，然后通过模板**自动生成**网页。

Jekyll 的优势也很明显：

- 完全免费：利用 GitHub Pages 免费托管，你无需购买域名和服务器。
- 生态成熟：拥有庞大的主题库，你可以一键选择风格，无需从零设计 UI。
- 自适应布局：网站在电脑和手机端都表现优秀，而你无需担心代码。
- 专注内容：你只需要写 Markdown 文件，无需关心复杂的 HTML/CSS 结构。

如果你不会写 Markdown，请不要担心，最多半小时，你就能掌握它。

如有需要，请参考这个教程网站：https://markdown.com.cn/basic-syntax/

## 如何利用 Jekyll？

打造作品集的第一步是选择外观（或者说“模版”）。

你不必花费数周设计界面，直接套用成熟的主题即可。

### 挑选主题

你可以从[Jekyll 官方网站](https://jekyllrb.com/docs/themes/)中找到6个官方推荐的主题网站。

我推荐下面2个：
- https://jekyllthemes.io/free
- http://jekyllthemes.org/

下面几个作品集主题模板我觉得还不错，仅供参考：
- https://jekyllthemes.io/theme/agency-jekyll-theme
- https://jekyllthemes.io/theme/freelancer-theme
- http://jekyllthemes.org/themes/portfolio-jekyll-theme/
- http://jekyllthemes.org/themes/hcz-jekyll-material/
- http://jekyllthemes.org/themes/PlainWhite-Jekyll/
- http://jekyllthemes.org/themes/cc-urban/

你现在看到的这个网站用的就是 http://jekyllthemes.org/ 上的 [Zolan 主题](http://jekyllthemes.org/themes/zolan/)。

如果你想对选择的主题做更多的自定义，你可以改脚本，不过这往往要花费更多的时间，好在现在你总可以请 AI 帮忙。

你现在看到的这个网站，在 Zolan 主题的基础上添加了搜索（右上角搜索按钮）、文章目录和快速回到上方页面功能（右下角的向上箭头）。

### 快速搭建与部署

选定了主题后，搭建过程非常简单。每个主题通常都会提供详细的使用指南，按照步骤一步步操作即可。

你通常需要做下面几件事：

1. 下载代码库到本地（下载压缩包或Git Clone）。
1. 在 GitHub 上创建一个仓库。
1. 本地修改配置和具体内容后，将本地代码库推送到上一步创建的仓库中。
1. 打开 GitHub Pages 设置。
1. 稍等几分钟后访问链接，就能看到你的网站了。

你通常需要更改以下文件或文件夹中的内容：
- `_config.yml`（配置文件）
- `_posts/`（博文所在处，如果你要上传自己的学习笔记，通常在这里）
- `_layouts/`（页面的不同模块）
- `_includes/`（页面的不同模块）

如果不知道怎么修改，搜索关键词可以帮你快速定位文件位置。

如果还是不会修改，问 AI，让它帮助你！

## 写在最后

当你把课程作业、毕业设计、学习笔记源源不断地上传并展示出来时，你收获的不仅仅是一个网址，更是一份属于你的个人作品集。

现在，就去 GitHub 上开始你的搭建之旅吧！期待在不久的将来，看到你的作品集助你发光发热。

也欢迎你在评论区留下你的网站链接，请大家观赏。

--- 