---
layout: post
title: "第三方 GitHub Action 突然 Docker build failed，如何稳定迁移到官方 Action？"
date: 2026-10-10 16:00:00 +0800
description: "本地 Jekyll 构建正常，但 GitHub Pages 部署突然 Docker build failed？本文记录从长期未更新的第三方 Action sujaykundu777/jekyll-deploy-action 迁移到 GitHub Pages 官方 GitHub Actions 的完整排查与解决步骤，不再依赖 gh-pages 分支和第三方 Docker 镜像。"
image: /images/jekyll-deploy-docker-build-failed.webp
image_alt: "Jekyll Docker build failed with exit code 1 报错截图"
tags: jekyll
---

如果你也在用 **Jekyll 4.x** 写博客，并且用第三方 GitHub Action 将 Jekyll 网站部署到 GitHub Pages 上，那你可能会遇到这种情况：

1. 本地 `jekyll serve` 一切正常，但 `git push` 之后网站却没更新；
1. 收到 GitHub 发来的 `Run failed: Jekyll Deploy - main` 邮件；
1. 点击邮件中的 View workflow run 后显示 `Docker build failed with exit code 1`

![{{page.image_alt}}]({{ site.baseurl }}{{page.image}})
*Docker build failed with exit code 1 报错截图*

如果你最近遇到了上面这种情况，别慌，本文正好能帮你省掉排查时间。

## 为什么第三方 Action 突然部署失败？

第三方 Action **维护可能不及时**，它们依赖的底层 Action 或运行环境可能在新环境里**失效**。

拿我用的第三方 Action [`sujaykundu777/jekyll-deploy-action@1.0.3`](https://github.com/sujaykundu777/jekyll-deploy-action){:target="_blank"} 来说，2026年9月5日出现 [`Docker build failed`](https://github.com/xiangxinotes/xiangxinotes.github.io/actions/runs/33954068498){:target="_blank"} 报错后，我才发现这个 Action 最新版本是 1.0.5，而且已经2年没有更新过了。

我先试着把版本从 1.0.3 升到 1.0.5，还是失败。到这个程度，我基本确认这个第三方 Action 已经不能用了，于是决定一次到位：**直接迁移到 GitHub Actions 官方 Pages 部署**。

## 迁移到官方 GitHub Actions 的步骤

下面是我当天使用的修复步骤。推荐按这个顺序操作：

1. 检查 `_config.yml` 有没有自定义 `destination`，有就删或注释（本地改动）；
2. 确认 `Gemfile.lock` 已提交，没有就补上（本地改动）；
3. 把 `.github/workflows/` 中 `yml` 文件的内容换成官方版（本地改动）；
4. 在 GitHub 仓库 `Settings -> Pages` 把 Source 改成 `GitHub Actions`（GitHub 设置）；
5. `git push` 一次推上去。

前三步都是本地文件改动，可以等到第4步之后再一起 commit 和 push，这样就不用手动 Re-run 了。

### 检查 `_config.yml` 有没有自定义 `destination`

官方 GitHub Actions 部署默认会把构建结果输出到 `_site`。如果你用的第三方 Action 在 `_config.yml` 里配置了**自定义 `destination`**（比如 `./build`），建议**删掉或注释**掉它，让产物统一输出到 `_site`。

比如我用的 `sujaykundu777/jekyll-deploy-action`，用 [./build 作为自定义 destination](https://github.com/sujaykundu777/jekyll-deploy-action#example-usage){:target="_blank"}，那么打开 `_config.yml`，找到：

```yaml
destination: ./build
```

删除或注释掉它。

官方 build action 其实也支持自定义输出目录，只要你把 `jekyll-build-pages` 和 `upload-pages-artifact` 两个 action 的输入改成一致即可。具体操作可参考 [GitHub Actions 官方指南](https://github.com/actions/jekyll-build-pages#usage){:target="_blank"}。但删掉更省事，也少一处容易造成错误的地方。

### 确认 `Gemfile.lock` 已提交

先确认远端仓库里有 `Gemfile.lock`。如果有，这一步可以跳过。如果没有，那你的 `.gitignore` 里大概率有 `Gemfile.lock` 这一行，把它删掉。（没找到 `.gitignore` 也不要慌，因为它默认是隐藏状态，根据你的操作系统，将其显示出来即可。）

然后在本地执行：

```bash
bundle install
bundle exec jekyll build
```

`Gemfile.lock` 会锁定每个 gem 的精确版本，保证你本地和 GitHub Actions 自动构建时用的是同一套依赖，避免本地成功、线上失败这种情况。

### 把 workflow 换成官方部署方式

进入 `.github/workflows/` 目录，找到正在使用的 `.yml` 文件，参考[官方用例](https://github.com/actions/jekyll-build-pages#usage){:target="_blank"}替换其中的内容。（没找到 `.github` 也别慌，因为它默认是隐藏状态，根据你的操作系统，将其显示出来即可。）

建议 `.github/workflows/` 目录下**只保留一个 `.yml` 文件**。如果之前有多个 yml，建议删除后只保留一个，再替换其中内容。

⚠️ 需要注意：官方示例的 build 步骤用的是 `actions/jekyll-build-pages@v1`。

但这个 action 自带 `github-pages` gem，而 `github-pages` gem 固定了 **Jekyll 3.10.0**，如果直接使用 `actions/jekyll-build-pages@v1` ，那还是会构建失败，你会看到这样的[日志](https://github.com/xiangxinotes/xiangxinotes.github.io/actions/runs/33962455372){:target="_blank"}内容：

```text
GitHub Pages: github-pages v232
GitHub Pages: jekyll v3.10.0
Theme: jekyll-theme-primer
...
EntryFilter: excluded /Gemfile.lock
```

![build 报错日志中显示 Gemfile.lock 被排除]({{ site.baseurl }}/images/jekyll-deploy-official-action-excluded-gemfile-lock.webp)
*build 报错日志 `EntryFilter: excluded /Gemfile.lock`*

日志里的 `EntryFilter: excluded /Gemfile.lock` 说明官方 action **不会读取我们仓库里的 `Gemfile.lock`**，它始终用自带的 Jekyll 3.10.0 来构建，这就和我们 `Gemfile.lock` 里锁定的 4.x 冲突。

解决办法是把 build 步骤换成直接运行：

```yaml
- name: Build with Jekyll
  run: bundle exec jekyll build --destination ./_site
```

还要**删掉 `actions/configure-pages@v5`** 那一步。

这样构建才会用你 `Gemfile.lock` 里锁定的 Jekyll 版本，和本地完全一致。

我最终使用的版本如下：

{% raw %}
```yaml
name: Deploy Jekyll site to Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.3'        # 与你本地一致
          bundler-cache: true        # 会自动使用 Gemfile.lock

      - name: Build with Jekyll
        run: bundle exec jekyll build --destination ./_site

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
{% endraw %}

### 把 GitHub Pages 的 Source 改成 GitHub Actions

之后就需要切换部署模式的开关。步骤如下：

打开 GitHub 仓库页面：

```text
Settings -> Pages -> Build and deployment
```

把 Source 从原来的：

```text
Deploy from a branch
```

改成：

```text
GitHub Actions
```

![GitHub Pages 的 Source 设置为 GitHub Actions 后的截图]({{ site.baseurl }}/images/jekyll-deploy-pages-source-github-actions.webp)
*Settings → Pages → Source 改成 GitHub Actions 后的状态*

旧 `gh-pages` 分支可以保留不管，也可以删掉，因为官方 Actions 部署已经不再依赖它了。

改成 `GitHub Actions` 后，下次 push 就会自动触发新的官方 workflow。

### push 所有改动

最后我们只需要把前三步的本地改动一次性提交：

```bash
git add _config.yml Gemfile.lock .github/workflows/main.yml
git commit -m "migrate to official GitHub Actions for Pages deployment"
git push
```

push 之后，新的官方 workflow 会自动触发。

## 验证是否成功

`git push` 后，如果没收到失败邮件，基本就是成功了。不过更稳妥的方式是进仓库的 `Actions` 标签页，查看最近一次 Job 的运行结果，`build` 和 `deploy` 都是绿色对勾，就说明部署成功。像这样：
[https://github.com/xiangxinotes/xiangxinotes.github.io/actions/runs/33962972898](https://github.com/xiangxinotes/xiangxinotes.github.io/actions/runs/33962972898){:target="_blank"}

---

把部署链路从**长期未更新的第三方 Action** 切换到 **GitHub Pages 官方推荐的 GitHub Actions** 以后，只要本地 `bundle exec jekyll build` 能通过，那 GitHub 上的构建就不会再出问题了。

对于需要维护个人博客的我们来说，这会让部署更稳定，也更省时省事。