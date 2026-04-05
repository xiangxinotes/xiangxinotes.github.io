---
layout: post
title: 震惊！解决 GitHub Pages 使用 Jekyll 主题时 tag 目录出现的 404 NOT FOUND 问题竟然如此简单！
date: 2023-04-28 17:27:35 +0800
description: 用 GitHub Pages 搭建个人网站，Jekyll theme 都选好了，但在自己的 GitHub Pages 出现了 tag 目录404？别担心，向西来帮你！
image: xiangxi-forked-zolan-tag-404-not-found.webp
tags: github-pages jekyll
---

学了好多知识，想在自己的博客上展示，选来选去选了 GitHub pages，连 Jekyll theme 都选好了，但在自己的 GitHub pages 出现了错误？！tag 目录404？向西来帮你！搞好了在简历上加一笔！绝对的提分儿项！

## 根本原因

你用的主题使用了 GitHub Pages **不支持**的插件！完整的支持插件列表在这里：[GitHub Pages 支持的插件](https://pages.github.com/versions/)。

不知道你用的主题用了那些插件？步骤如下：
1. 最外层目录下，打开 **`_config.yml`**
![向西的 GitHub 代码仓库 Zolan 最外层目录]({{ site.baseurl }}/images/xiangxi-forked-zolan-working-directory.webp)
*最外层目录下有一个 _config.yml*
2. 搜索 **`plugins:`** 其下就是该主题所用插件。
![Zolan 主题所用的插件截图]({{ site.baseurl }}/images/xiangxi-forked-zolan-config-yml.webp)
*Zolan 主题所用的插件*

由上可知，Zolan 主题使用插件如下：
  - `jekyll-paginate`
  - `jekyll-sitemap`
  - `jekyll/tagging`

可以看出 Zolan 用 `jekyll/tagging` 来处理 `tag`，而 `jekyll/tagging` 是 GitHub Pages 不支持的插件，因为它不在这个 [GitHub Pages 支持的插件列表](https://pages.github.com/versions/)里。

## 解决方法

既然 GitHub Pages 不支持这个插件，有办法实现想要的效果吗？官方给出了答案：[当然](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll#plugins)！你只要本地生成 site 文件，然后把 tag/ 文件夹 `push` 到 GitHub 上就可以了。

还是不知如何操作？下面我以zolan为例，带你走完全程！

### 在 Terminal 输入以下指令：
1. `git clone https://github.com/xiangxinotes/zolan.git`
1. `bundle install`
1. `jekyll serve`

以上指令均成功后，会出现提示：
```terminal
Server address: http://127.0.0.1:4000/
Server running... press ctrl-c to stop.
```
此时可本地查看你的网站：http://127.0.0.1:4000/

### 本地工作目录下多了一个 _site/ 的文件夹，执行以下操作：
1. 复制 _site/ 文件夹内的 tag/ 文件夹
![向西笔记本地目录 _site/ 文件夹详情]({{ site.baseurl }}/images/xiangxinotes-github-io-site-folder-under-working-directory.webp)
*_site/ 文件夹内的 tag/ 文件夹*
1. 粘贴到最外层目录下。
![向西笔记本地工作目录全貌]({{ site.baseurl }}/images/xiangxinotes-github-io-tag-folder-under-working-directory.webp)
*最外层目录下的 tag/ 文件夹*

### 回到 Terminal ，输入以下指令：
1. `git add .`
2. `git commit -m "Upload static tag folder"`
3. `git push -u origin main`

等 `push` 生效后查看，tag 页面没有404报错了，问题解决！

⚠️ 注意：添加新的 post 记得删除最外层目录下的 tag/ 文件夹，把新的 _site/tag/ 复制粘贴过去。( yeah, I know, troublesome 😮‍💨

## 参考资料
- [向西 forked Zolan theme 无 tag/ 文件夹](https://xiangxinotes.github.io/zolan/)
- [向西 GitHub Pages 有 tag/ 文件夹](https://github.com/xiangxinotes/xiangxinotes.github.io)
- [GitHub Pages 支持的jekyll插件](https://pages.github.com/versions/)
- [GitHub Pages 如何使用不支持的插件](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll#plugins)
- [Zolan 主题线上预览](https://zolan-jekyll.netlify.app/)
- [Zolan 主题页](http://jekyllthemes.org/themes/zolan/)
- [jekyll themes](http://jekyllthemes.org/)


---

问题解决了吗？开心不？有成就感不？如果有帮到你，哪怕一丢丢，就把这篇文章分享给其他遭遇困难的朋友，一起升级打怪吧！～ ❤️

下面是此文章面世的原因，如果你感兴趣，继续往下看👇

## 背景 Context

此网站 hosted on [GitHub](https://www.github.com), powered by [Jekyll](https://github.com/jekyll/jekyll) and based on [Zolan theme](http://jekyllthemes.org/themes/zolan/).

当我使用[Zolan theme](http://jekyllthemes.org/themes/zolan/)时，我也遇到了和你一样的404。刚开始我也和你一样崩溃，要是一切顺利多好，是吧。可惜，天不遂人愿。

我遇到的 404 Page Not Found 问题：
1. 在 GitHub 上 fork [Zolan 主题](http://jekyllthemes.org/themes/zolan/)，打开 GitHub pages 设置。
![向西 fork 的 Zolan 主题截图]({{ site.baseurl }}/images/xiangxi-forked-zolan.webp)
*在 GitHub 上 fork Zolan 主题*
2. 切换至 Settings 标签，左侧面板选择 Pages。右侧 Build and deployment，Source 选择 Deploy from a branch，Branch 选择 master, /(root)。稍等1～2分钟，GitHub Pages 下出现 site link，点击 Visite site 来打开它
![GitHub Pages 设置截图]({{ site.baseurl }}/images/xiangxi-forked-zolan-github-pages.webp)
*GitHub Pages 设置*
3. 在 LATEST POSTS 中随便点击一个 post 的 tag。
![点击 tag 为 resources 的标签]({{ site.baseurl }}/images/xiangxi-forked-zolan-resources-tag-under-latest-posts.webp)
*随便点击一个 post 的 tag，比如 resources*
4. tag 页面显示 404 Page not found
![显示404 Page not found的tag 页面]({{ site.baseurl }}/images/xiangxi-forked-zolan-tag-404-not-found.webp)
*tag 页面显示 404 Page not found*
