---
layout: page
title: "订阅向西笔记" 
description: "订阅《向西笔记》Newsletter。每周为你送上关于沟通技巧、职场思考与自我管理的原创干货。无垃圾邮件，随时可退订。"
date: 2026-10-27 23:19:00 +0800
permalink: /newsletter/
menu_nav: false
hide_title: true
---

<div class="newsletter-page">
  <div class="newsletter-header">
    <h2>保持联系，一起成长</h2>
    <p>在这个信息爆炸的时代，我承诺只把最有价值的内容送到你的邮箱。<br>每周一篇原创文章，关于沟通技巧、职场思考与自我管理。没有垃圾邮件，随时可以退订。</p>
  </div>
  <div class="subscribe-wrapper">
    <form class="subscribe-form validate" action="{% if site.data.settings.mailchimp %}//{{site.data.settings.mailchimp}}{% else %}#{% endif %}"
      method="POST" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" target="_blank" novalidate>
      <div class="subscribe-group">
        <label class="screen-reader-text" for="mce-EMAIL">邮件地址</label>
        <input class="subscribe__email required email" id="mce-EMAIL" type="email" name="EMAIL" placeholder="你的邮箱" required>
      </div>
      <input class="subscribe__button" id="mc-embedded-subscribe" type="submit" name="subscribe" value="免费订阅">
    </form>
    <p class="privacy-note">提交即表示同意订阅。邮箱数据安全存储于 Mailchimp，随时可退订。详情请参阅<a href="/privacy/">隐私政策</a>。</p>
  </div>
</div>