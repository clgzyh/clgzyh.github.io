---
layout: default
title: Categories
---


<div id="archives">
  {% for category in site.categories %}
    {% assign category_name = category | first %}
    {% if category_name != "cheatsheet" %}
      <!-- 检查这个分类下是否至少有一篇不是 cheatsheet 的文章 -->
      {% assign has_normal_post = false %}
      {% for post in site.categories[category_name] %}
        {% if post.category != "cheatsheet" and post.categories contains "cheatsheet" == false %}
          {% assign has_normal_post = true %}
        {% endif %}
      {% endfor %}

      {% if has_normal_post %}
        <div class="archive-group">
          <div id="#{{ category_name | slugize }}"></div>
          <h4 class="category-head">{{ category_name }}</h4>
          {% for post in site.categories[category_name] %}
            {% if post.category != "cheatsheet" and post.categories contains "cheatsheet" == false %}
              <article class="archive-item">
                <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
                <a style="text-decoration:none" href="{{ post.url }}">{{ post.title }}</a>
              </article>
            {% endif %}
          {% endfor %}
        </div>
      {% endif %}
    {% endif %}
  {% endfor %}
</div>