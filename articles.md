---
layout: default
title: Articles
---


<div id="articles">
  <h1>文章</h1>
  <ul class="posts noList">
    {% for post in site.posts %}
      {% if post.categories contains "cheatsheet" == false %}
        <li>
          <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
          <a href="{{ post.url }}">{{ post.title }}</a>
        </li>
      {% endif %}
    {% endfor %}
  </ul>
</div>