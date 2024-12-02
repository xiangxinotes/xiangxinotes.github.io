---
layout: post
title: 【详细教程】怎么解决 javascript 访问本地文件时的浏览器跨域报错，实现本地预览？A step to step guide!
date: 2024-12-02 12:14:13 +0800
description: 在 js 中访问本地文件（可以是任何文件，但通常是 json 或 js 文件），浏览器却提示：Access to fetch at 'file:///.../xx.json' from origin 'null' has been blocked by CORS policy. Cross origin requests are only supported for protocol schemes. chrome, chrome-extension, chrome-untrusted, data, http, https, isolated-app. GET file:///../xx.json net::ERR_FAILED. Uncaught (in promise) TypeError. Failed to fetch. 本文分享2个方法助你轻松解决报错，实现本地预览！
image: access-to-fetch-at-file-from-origin-null-has-been-blocked-by-CORS-policy.png
tags: javascript js json local-file cross-origin local-preview
---

在前端开发的过程中，我们时常会遇到需要在 javascript 中直接访问本地 json 文件、js 文件（当然也可以是其他文件）的情况，我们写好了脚本，但双击 html 后却常常会从浏览器的控制台中看到这样的报错提示：

![Chrome浏览器 blocked by CORS policy, net::ERR_FAILED, Failed to fetch 报错截图]({{ site.baseurl }}/images/uncaught-in-promise-typerror-failed-to-fetch.png)
*Chrome浏览器 blocked by CORS policy, net::ERR_FAILED, Failed to fetch 报错截图*

```javascript
Access to fetch at 'file:///.../xx.json' from origin 'null' has been blocked by CORS policy: Cross origin requests are only supported for protocol schemes: chrome, chrome-extension, chrome-untrusted, data, http, https, isolated-app. 

GET file:///../xx.json net::ERR_FAILED. 

Uncaught (in promise) TypeError: Failed to fetch
    at functionName (script.js)
    at window.onload (script.js)
```

本文会告诉你导致这类报错的根本原因，以及如何解决这类报错（实际是达成你本地预览的需求）。本文将为你提供一份详细的 step by step 指南，帮助你轻松搞定此事。

## 根本原因

会出现这类报错的根本原因其实在报错提示中就告知了，我来翻译下：

```javascript
从 'null' 的域处获取 'file:///.../xx.json' 的访问已被CORS 策略阻止: 跨域请求仅支持以下协议方案: chrome, chrome-extension, chrome-untrusted, data, http, https, isolated-app.

GET 文件:///.../xx.json net::ERR_FAILED。

未捕获 (在 promise 中) TypeError: 获取失败
    在 functionName 中 (script.js)
    在 window.onload 中 (script.js)
```

因为我们想要跨域访问的本地文件是以 'file://' 开始的，也就是说我们使用的协议方案是 **file**。file 并不在跨域请求支持的协议方案（chrome、chrome-extension、chrome-untrusted、data、http、https、isolated-app）里。所以通过 'file://' 我们是没办法访问本地文件的。

既然不能通过 'file://' 访问本地文件的，我们就换一种方法即可。

## 解决方案

如果你可以在当前的电脑上安装软件，或者已经安装了 VS Code，那么使用[方案一]({{ page.url }}/#%E6%96%B9%E6%A1%88%E4%B8%80%E4%BD%BF%E7%94%A8-vs-code-%E4%B8%AD%E7%9A%84%E6%8F%92%E4%BB%B6%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E9%A2%84%E8%A7%88)对你更方便省事。
如果你不能在当前电脑中随意安装软件（公司控制）或者你不想安装 VS Code，那么建议你使用[方案二]({{ page.url }}/#%E6%96%B9%E6%A1%88%E4%BA%8C%E8%AE%BF%E9%97%AE%E6%89%98%E7%AE%A1%E5%B9%B3%E5%8F%B0%E4%B8%AD%E7%9A%84%E6%96%87%E4%BB%B6%E6%9D%A5%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E9%A2%84%E8%A7%88)。

### 方案一：使用 VS Code 中的插件实现本地预览

既然我们不能通过 'file://' 访问本地文件，那我们就搭建一个本地服务器来测试，这样的话，我们就能使用 http 协议访问同一个资源了。

搭建一个本地服务器最简单的方法只需要下面4步：
1. 安装 VS Code。[VS Code下载链接🔗](https://code.visualstudio.com/Download)
2. 安装 [Live Server 插件](https://github.com/ritwickdey/vscode-live-server-plus-plus)
  - 点击主界面左下角的设置按钮（参考下方截图1）
  - 选择 Extensions（参考下方截图1）
  - 在左上角输入并搜索 “Live Server”（参考下方截图2）
  - 选择 Ritwick Dey 的这个 “Live Server”（参考下方截图2）
  - 在右边点击 install（参考下方截图2）
3. 重启 VS Code
4. 点击 VS Code 右下角的 Go Live 实现本地预览（参考下方截图3）

![VS Code 界面截图1: 点击主界面左下角的设置按钮、选择 Extensions]({{ site.baseurl }}/images/click-settings-button-then-choose-extensions-to-install-live-server-in-vs-code.png)
*VS Code 界面截图1：点击主界面左下角的设置按钮、选择 Extensions*

![VS Code 界面截图2: 在左上角输入并搜索 “Live Server”、选择 Ritwick Dey 的这一个 “Live Server”、在右边点击 install]({{ site.baseurl }}/images/input-select-and-install-the-extension-live-server.png)
*VS Code 界面截图2：在左上角输入并搜索 “Live Server”、选择 Ritwick Dey 的这一个 “Live Server”、在右边点击 install*

![VS Code 界面截图3: 重启后点击 VS Code 右下角的 Go Live 实现本地预览]({{ site.baseurl }}/images/click-go-live-to-build-a-local-server.png)
*VS Code 界面截图3：重启后点击 VS Code 右下角的 Go Live 实现本地预览*

### 方案二：访问托管平台中的文件来实现本地预览

既然我们不能通过 'file://' 访问本地文件，而我们又不能或者不想在当前电脑中随意安装软件，那我们就把本地文件上传到某个可以托管文件的平台（比如 Github），这样的话，我们就能使用 http 或 https 协议来访问资源了。

使用这个方案，需要做以下3步：
1. 把文件上传到某个可以托管文件的平台上去，
2. 获得可以访问到这个文件的链接，
3. 把 js 文件中的链接改为第2步的链接。

上面3步完成以后，你在本地直接双击 html 即可在浏览器预览了。

托管文件的平台很多，如果你准备用 Github，但不知道怎么做上面的第2步：获得可以访问到这个文件的链接，那么请参考这一篇博文：[怎么直接访问并使用托管在 Github 上的 json 文件？](/2024/11/28/how-to-access-or-use-json-files-that-hosted-on-github/)，里面有详细的步骤。

如果因为公司控制等各种原因你不能把一模一样的文件上传到某个可以托管文件的平台，那你可以上传一些结构相似的测试内容来代替。只是为了在本地查看效果的话，这样就足够了，你觉得呢？

如果你想要了解这篇博文面试的原因，请继续往下读～

---

## 背景 Context

最近这2周，我做了一个 [SQL 语句复习网站：SQL Wizland](/sql-wizland)。因为我发现，虽然我一直觉得 SQL 很简单，也在 Leetcode, StrataScratch 上面刷了很多题，但是一段时间不用，就还是会把这些简单的 SQL 语句忘掉。为了克服这个困难，也为了帮助和我有同样困扰的朋友，我做了这个网站：[SQL Wizland](/sql-wizland)。

网站的数据主要包含2个方面：
- 语句类型
- 题目

在这个网站的制作过程中，为了更好的管理这2个方面的数据，我把 js 由原来的 script.js 变为了：
- script.js
- json/catogries.json
- json/questions.json

其中的两个json文件就对应了网站的两方面数据。

这就需要我在 script.js 中 fetch 这两个 json 文件了，也就是在更改之后才出现了本地预览的问题。前期我一直都在[方案二]({{ page.url }}/#%E6%96%B9%E6%A1%88%E4%BA%8C%E8%AE%BF%E9%97%AE%E6%89%98%E7%AE%A1%E5%B9%B3%E5%8F%B0%E4%B8%AD%E7%9A%84%E6%96%87%E4%BB%B6%E6%9D%A5%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E9%A2%84%E8%A7%88)，但每次 push 到 Github 仓库时都要改来改去实在麻烦，我就干脆改用[方案一]({{ page.url }}/#%E6%96%B9%E6%A1%88%E4%B8%80%E4%BD%BF%E7%94%A8-vs-code-%E4%B8%AD%E7%9A%84%E6%8F%92%E4%BB%B6%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E9%A2%84%E8%A7%88)了，省点时间。

看到这里的你，pick 哪一个方案呢？可以在评论区留言让我知道哦！
