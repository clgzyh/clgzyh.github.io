---
layout: default
title: Categories
---


<div id="archives">
  {% for category in site.categories %}
    {% assign name = category[0] %}
    {% if name != "cheatsheet" %}
      {% assign has_normal = false %}
      {% for post in category[1] %}
        {% unless post.categories contains "cheatsheet" %}
          {% assign has_normal = true %}
        {% endunless %}
      {% endfor %}

      {% if has_normal %}
        <div class="archive-group">
          <div id="#{{ name | slugize }}"></div>
          <h4 class="category-head">{{ name }}</h4>
          {% for post in category[1] %}
            {% unless post.categories contains "cheatsheet" %}
              <article class="archive-item">
                <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
                <a style="text-decoration:none" href="{{ post.url }}">{{ post.title }}</a>
              </article>
            {% endunless %}
          {% endfor %}
        </div>
      {% endif %}
    {% endif %}
  {% endfor %}
</div>