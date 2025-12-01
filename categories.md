---
layout: default
title: Categories
---


<div id="archives">
  {% for category in site.categories %}
    {% assign category_name = category | first %}
    {% assign has_normal_post = false %}
    
    <!-- 检查这个分类下是否有非速查手册文章 -->
    {% for post in site.categories[category_name] %}
      {% unless post.cheatsheet %}
        {% assign has_normal_post = true %}
      {% endunless %}
    {% endfor %}
    
    {% if has_normal_post %}
      <div class="archive-group">
        <div id="#{{ category_name | slugize }}"></div>
        <h4 class="category-head">{{ category_name }}</h4>
        {% for post in site.categories[category_name] %}
          {% unless post.cheatsheet %}
            <article class="archive-item">
              <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
              <a style="text-decoration:none" href="{{ post.url }}">{{ post.title }}</a>
            </article>
          {% endunless %}
        {% endfor %}
      </div>
    {% endif %}
  {% endfor %}
</div>