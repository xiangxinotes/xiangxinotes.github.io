---
layout: default
title: "精选效率模板与官方规范库" 
description: "向西笔记的私人工具箱。包含亲测有效的成功日记模板、沟通问题清单、梦想家行动指南等原创效率工具，以及免费分享的公共场所英文译写官方规范。附带详细的实战心法与保姆级教程。"
date: 2026-10-26 23:08:00 +0800
permalink: /templates/
image: /images/templates.webp
menu_nav: false
---

<main class="tag-page-main">
  <section class="tag-hero">
    <div class="hero-container">
      <h1 class="tag-title">精选模板与资源库</h1>
      <p class="tag-description">
        这里是我在践行个人管理、自我沟通与高效行动时，亲自沉淀下来的工具箱。<br>
        包含我亲手设计的<strong>原创工具</strong>（如成功日记、沟通清单，均附带背后的踩坑与方法论），以及我精心整理的<strong>权威官方资源</strong>。所有资源均可一键获取，帮你少走弯路，直接行动。
      </p>
    </div>
  </section>

  <!-- 卡片网格区域 -->
  <section class="articles-section">
    <div class="articles-container">
      <!-- 排序逻辑开始 -->
      {% assign sortable_templates = "" | split: "" %}
      
      {% for tmpl in site.data.templates %}
        {% assign update_ts = 0 %}
        <!-- 去文章里找最后修改时间 -->
        {% if tmpl.guide_url %}
          {% for post in site.posts %}
            {% if post.url contains tmpl.guide_url %}
              {% assign update_ts = post.last_modified_at | default: post.date | date: "%s" | plus: 0 %}
              {% break %}
            {% endif %}
          {% endfor %}
        {% endif %}
        <!-- 如果没找到文章，用当前时间兜底 -->
        {% if update_ts == 0 %}
          {% assign update_ts = "now" | date: "%s" | plus: 0 %}
        {% endif %}
        
        <!-- 把时间戳和序号拼起来便于排序 -->
        {% assign entry = update_ts | append: "|" | append: forloop.index0 %}
        {% assign sortable_templates = sortable_templates | push: entry %}
      {% endfor %}
      
      {% assign sorted_entries = sortable_templates | sort | reverse %}
      
      <!-- 渲染排序后的卡片 -->
      {% for entry in sorted_entries %}
        {% assign parts = entry | split: "|" %}
        {% assign idx = parts[1] | plus: 0 %}
        {% assign tmpl = site.data.templates[idx] %}
        {% include template-card.html template=tmpl %}
      {% endfor %}
      <!-- 排序逻辑结束 -->
      
    </div>
  </section>
</main>