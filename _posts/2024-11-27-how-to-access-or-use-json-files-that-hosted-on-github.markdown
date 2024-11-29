---
layout: post
title: 【详细教程】怎么直接访问并使用托管在 Github 上的 json 文件？A step to step guide!
date: 2024-11-28 14:56:35 +0800
description: 想要把 Github 当做自己的 json 库，减轻自己服务器的压力？想要通过 http 请求获取 Github 中 json 文件的内容？本文提供详细的分步指南，包括精准定位文件存放位置以及对其进行有效加工的方法，附实际演示案例，助你轻松上手。
image: categories-json-in-sql-wizland-hosted-on-github.png
tags: github json
---

在开发过程中，时常会遇到需要直接访问并使用托管在 Github 上的 json 文件的情况，本文将为你提供一份详细的 step by step 指南，帮助你轻松搞定此事。

## 方法

### 1. 精准定位：得到 json 文件在 Github 中的存放位置

想要访问并使用一个托管在 Github 上的 json 文件，你首先需要得到它在 Github 中的存放位置。别担心，我会带着你一起找。

首先，这个 json 文件一定存在于某个 Github 用户（这个用户可以是你自己，也可以是别人）的**某个 Github 仓库中**，所以你要做的就是：
1. 在 Github 的网站上找到这个 Github 仓库。
2. 在这个 Github 仓库中找到这个 json 文件。如果这个文件在其他文件夹里，请逐级打开，直到你找到这个 json 文件。
3. 点击这个 json 文件的名字打开它，请检查浏览器显示的网址，结尾应该是.json
4. 复制上一步的网址。

#### 演示案例：

演示以访问 xiangxinotes 用户在 sql-wizland 仓库中的 categories.json 文件为例，详细展示如何直接访问并使用 Github 上的这类 json 文件。

步骤是：
1. 在 Github 的网站上找到 [xiangxinotes/sql-wizland 这个 Github 仓库](https://github.com/xiangxinotes/sql-wizland/)
2. 在这个 Github 仓库中找到了 [categories.json 文件](https://github.com/xiangxinotes/sql-wizland/tree/main/json)，位于json文件夹内。
3. 点击这个 json 文件的名字打开它，[浏览器显示的网址](https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json)结尾是.json
4. 复制上一步的网站：[https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json](https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json)

![categories.json 文件在名为 xiangxinotes 的 Github 用户的名为 sql-wizland 的 Github 仓库中的截图展示]({{ site.baseurl }}/images/categories-json-in-sql-wizland-hosted-on-github.png)

### 2. 关键步骤：对 json 文件在 Github 中的存放位置进行加工

接下来，我们要对[第1步]({{ page.url }}/#1-%E7%B2%BE%E5%87%86%E5%AE%9A%E4%BD%8D%E5%BE%97%E5%88%B0-json-%E6%96%87%E4%BB%B6%E5%9C%A8-github-%E4%B8%AD%E7%9A%84%E5%AD%98%E6%94%BE%E4%BD%8D%E7%BD%AE)获得的链接进行加工，因为当你在本地通过 fetch 直接使用这个链接的时候，会有2条报错：

```
GET https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json net::ERR_CONNECTION_REFUSED

Uncaught (in promise) TypeError: Failed to fetch
  at populate (script.js:11:26)
  at window.onload (script.js:263:3)
```

![使用第一步链接在本地直接 fetch 的2条报错展示]({{ site.baseurl }}/images/fetch-json-from-front-end-url-error.png)

为了解决这两条报错，我们要对这个链接进行加工，有两种加工方法。不过在加工前我们需要先了解一下我们在[上一步]({{ page.url }}/#1-%E7%B2%BE%E5%87%86%E5%AE%9A%E4%BD%8D%E5%BE%97%E5%88%B0-json-%E6%96%87%E4%BB%B6%E5%9C%A8-github-%E4%B8%AD%E7%9A%84%E5%AD%98%E6%94%BE%E4%BD%8D%E7%BD%AE)获得的链接结构。

#### 认识 json 文件在 Github 中的存放位置 链接的结构：

我们在第一步得到的网址其实满足这样的格式：**https://github.com/${github-account-name}/${repository-name}/blob/${branch-name}/../${json-file-name}.json**

其中：
- `${github-account-name}` 是某个 Github 用户的用户名，
- `${repository-name}` 是这个 Github 用户的某个 Github 仓库名，
- `${branch-name}` 是这个 Github 用户的这个 Github 仓库里的某个分支名，
- `../` 部分表示目录结构，
- `${json-file-name}` 是这个 Github 用户的这个 Github 仓库里的这个分支里的某个 json 文件的文件名。

我们通过一个具体的链接分析一下结构：
**https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json**

其中：
- `xiangxinotes` 对应 `${github-account-name}`
- `sql-wizland` 对应 `${repository-name}`
- `main` 对应 `${branch-name}`
- `json/` 对应 `../`
- `categories.json` 对应 `${json-file-name}` 

认识了链接结构之后，我们可以开始加工链接了，两种方法：

#### 加工成 raw.githubusercontent.com 的链接格式

raw.githubusercontent.com 以原始格式提供存储在 GitHub 仓库中的文件内容。通过这个域名可以直接获取这些文件的原始内容，而不是经过 HTML 渲染之后的内容。

一个正确的加工后链接格式应该是这样的：
**https://raw.githubusercontent.com/${github-account-name}/${repository-name}/${branch-name}/../${json-file-name}.json**

这种方法有一个缺点：国内或者国内某些地区访问 raw.githubusercontent.com 速度很慢，有时甚至会出现无法访问的情况。

##### 演示案例：

第一步获得的链接是[https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json](https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json)

其中：
- `xiangxinotes` 对应 `${github-account-name}`
- `sql-wizland` 对应 `${repository-name}`
- `main` 对应 `${branch-name}`
- `json/` 对应 `../`
- `categories.json` 对应 `${json-file-name}` 

我们对应后，把链接修改为：
[https://raw.githubusercontent.com/xiangxinotes/sql-wizland/main/json/categories.json](https://raw.githubusercontent.com/xiangxinotes/sql-wizland/main/json/categories.json)

![从 raw.githubusercontent.com 访问 categories.json 文件的截图]({{ site.baseurl }}/images/json-file-accessed-from-raw-githubusercontent-com.png)
*从 raw.githubusercontent.com 访问 categories.json 文件的截图*

#### 加工成 ${github-account-name}.github.io 的链接格式

${github-account-name}.github.io 是 名为 ${github-account-name} 的 Github 用户通过 Github Pages 服务创建的网站，也托管在 Github 上。我们可以通过 ${github-account-name}.github.io 直接访问其中的静态文件。

一个正确的加工后链接格式应该是这样的：
**https://${github-account-name}.github.io/${repository-name}/../${json-file-name}.json**

这种方法的优点完美解决了访问 raw.githubusercontent.com 速度慢的问题。但是如果这位用户并未使用 Github Pages 创建静态站，那么上面的链接可能没用。所以在使用这个方法前，请务必访问一下 ${github-account-name}.github.io 这个链接，注意一定要替换 ${github-account-name}。

##### 演示案例：

第一步获得的链接是[https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json](https://github.com/xiangxinotes/sql-wizland/blob/main/json/categories.json)

其中：
- `xiangxinotes` 对应 `${github-account-name}`
- `sql-wizland` 对应 `${repository-name}`
- `main` 对应 `${branch-name}`
- `json/` 对应 `../`
- `categories.json` 对应 `${json-file-name}` 

我们对应后，把链接修改为：
[https://xiangxinotes.github.io/sql-wizland/json/categories.json](https://xiangxinotes.github.io/sql-wizland/json/categories.json)

![从 xiangxinotes.github.io 访问 categories.json 文件的截图]({{ site.baseurl }}/images/json-file-accessed-from-github-io.png)
*从 xiangxinotes.github.io 访问 categories.json 文件的截图*

---

直接访问并使用托管在 Github 上的 json 文件了吗？开心不？有成就感不？如果按照步骤操作后还是无法访问 json 文件的话，请在评论区详细讲一下你遇到的问题，我会看看能不能帮到你。如果这篇文章如果有帮到你，哪怕一丢丢，就把这篇文章分享给其他遭遇困难的朋友，一起升级打怪吧！～ ❤️

下面是直接访问并使用托管在 Github 上的 json 的一些使用场景：

## 使用场景

- [MDN 在 Github 托管演示案例用到的 json 文件](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/JSON#%E5%BC%80%E5%A7%8B%E5%90%A7)
- 我创建了[一个 SQL 语句复习网站 SQL Wizland](/sql-wizland)，需要在本地直接访问托管在 Github 上的 json文件，绕过了本地访问文件跨域的报错

