---
layout: default
title: Categories
---


<div id="archives">
  {% for category in site.categories %}
    {% assign category_name = category | first %}
    {% if category_name != "cheatsheet" %}
      <!-- 只显示该分类下至少有一篇非 cheatsheet 文章的分类 -->
      {% assign has_normal_post = false %}
      {% for post in category[1] %}
        {% if post.categories contains "cheatsheet" == false %}
          {% assign has_normal_post = true %}
        {% endif %}
      {% endfor %}

      {% if has_normal_post %}
        <div class="archive-group">
          <div id="#{{ category_name | slugize }}"></div>
          <h4 class="category-head">{{ category_name }}</h4>
          {% for post in category[1] %}
            {% if post.categories contains "cheatsheet" == false %}
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