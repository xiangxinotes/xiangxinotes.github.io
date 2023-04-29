---
layout: post
title: 震惊！解决 Github pages 使用 jekyll 主题时 tag 目录出现的 404 NOT FOUND 问题竟然如此简单！
date: 2023-04-28 17:27:35 +0800
description: 用 Github pages 搭建个人网站，jekyll theme 都选好了，但在自己的 Github pages 出现了 tag 目录404？别担心，向西来帮你！
image: xiangxi-forked-zolan-tag-404-not-found.png
tags: Github-pages jekyll-themes
---

学了好多知识，想在自己的博客上展示，选来选去选了 Github pages，连 jekyll theme 都选好了，但在自己的 Github pages 出现了错误？！tag 目录404？向西来帮你！搞好了在简历上加一笔！绝对的提分儿项！

## 问题复现

本网站 hosted on Github, powered by jekyll and based on [Zolan theme](http://jekyllthemes.org/themes/zolan/).

当我使用[Zolan](http://jekyllthemes.org/themes/zolan/)这个主题时，我也遇到了和你一样的404。刚开始我也和你一样崩溃，要是一切顺利多好，是吧。可惜，天不遂人愿。

你可以通过以下步骤复现404 Page Not Found问题：
1. 在 Github 上 fork [Zolan 主题](http://jekyllthemes.org/themes/zolan/)，打开 Github pages 设置。
![向西 fork 的 Zolan 主题截图]({{ site.baseurl }}/images/xiangxi-forked-zolan.png)
*在 Github 上 fork Zolan 主题*
2. 切换至 Settings 标签，左侧面板选择 Pages。右侧 Build and deployment，Source 选择 Deploy from a branch，Branch 选择 master, /(root)。稍等1～2分钟，Github Pages 下出现 site link，点击 Visite site 来打开它
![Github Pages 设置截图]({{ site.baseurl }}/images/xiangxi-forked-zolan-github-pages.png)
*Github Pages 设置*
3. 在 LATEST POSTS 中随便点击一个 post 的 tag。
![点击 tag 为 resources 的标签]({{ site.baseurl }}/images/xiangxi-forked-zolan-resources-tag-under-latest-posts.png)
*随便点击一个 post 的 tag，比如 resources*
4. tag 页面显示 404 Page not found
![显示404 Page not found的tag 页面]({{ site.baseurl }}/images/xiangxi-forked-zolan-tag-404-not-found.png)
*tag 页面显示 404 Page not found*

## 根本原因

出现这个问题的根本原因是：你所使用的主题使用了 Github Pages **不支持**的插件。

完整的支持插件列表在这里：[Github Pages 支持的插件](https://pages.Github.com/versions/)。不知道怎么查看你所使用的主题使用了那些插件？步骤如下：
1. 最外层目录下，打开 **`_config.yml`**
![向西的 Github 代码仓库 Zolan 最外层目录]({{ site.baseurl }}/images/xiangxi-forked-zolan-working-directory.png)
*最外层目录下有一个 _config.yml*
2. 搜索 **`plugins:`** 其下就是该主题所用插件。
![Zolan 主题所用的插件截图]({{ site.baseurl }}/images/xiangxi-forked-zolan-config-yml.png)
*Zolan 主题所用的插件*

由上可知，Zolan 主题使用插件如下：
  - `jekyll-paginate`
  - `jekyll-sitemap`
  - `jekyll/tagging`

可以看出 Zolan 用 `jekyll/tagging` 来处理 tag，而 `jekyll/tagging` 是 Github Pages 不支持的插件，因为它不在这个 [Github Pages 支持的插件列表](https://pages.Github.com/versions/)里。

## 解决问题

既然 Github Pages 不支持这个插件，有办法实现想要的效果吗？官方给出了答案：[当然](https://docs.Github.com/en/pages/setting-up-a-Github-pages-site-with-jekyll/about-Github-pages-and-jekyll#plugins)！你只要本地生成 site 文件，然后把 site 的静态文件 push 到 Github 上就可以了。

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
此时网站本地也可查看了：http://127.0.0.1:4000/

### 本地工作目录下多了一个 _site/ 的文件夹，执行以下操作：
1. 复制 _site/ 文件夹内的 tag/ 文件夹
![向西笔记本地目录 _site/ 文件夹详情]({{ site.baseurl }}/images/xiangxinotes-github-io-site-folder-under-working-directory.png)
*_site/ 文件夹内的 tag/ 文件夹*
1. 粘贴到最外层目录下。
![向西笔记本地工作目录全貌]({{ site.baseurl }}/images/xiangxinotes-github-io-tag-folder-under-working-directory.png)
*最外层目录下的 tag/ 文件夹*

### 回到 Terminal ，输入以下指令：
1. `git add .`
2. `git commit -m "Upload static tag folder"`
3. `git push -u origin main`

等 push 生效后查看，tag 页面没有404报错了，问题解决！

⚠️ 注意：添加新的 post 记得删除最外层目录下的 tag/ 文件夹，把新的 _site/tag/ 复制粘贴过去。( yeah, I know, troublesome 😮‍💨

## 参考资料
- [向西 forked Zolan theme 无 tag/ 文件夹](https://xiangxinotes.Github.io/zolan/)
- [向西 Github Pages 有 tag/ 文件夹](https://Github.com/xiangxinotes/xiangxinotes.Github.io)
- [Github Pages 支持的jekyll插件](https://pages.Github.com/versions/)
- [Github Pages 如何使用不支持的插件](https://docs.Github.com/en/pages/setting-up-a-Github-pages-site-with-jekyll/about-Github-pages-and-jekyll#plugins)
- [Zolan 主题线上预览](https://zolan-jekyll.netlify.app/)
- [Zolan 主题页](http://jekyllthemes.org/themes/zolan/)
- [jekyll themes](http://jekyllthemes.org/)


---

问题解决了吗？开心不？有成就感不？如果有帮到你，哪怕一丢丢，就把这篇文章分享给其他遭遇困难的朋友，一起升级打怪吧！～ ❤️
