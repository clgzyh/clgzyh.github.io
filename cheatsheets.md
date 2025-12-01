---
layout: default
title: Cheatsheets
---


<div id="articles">
  <h1>速查手册</h1>
  <ul class="posts noList">
    {% for post in site.posts %}
      {% if post.cheatsheet %}
        <li>
          <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
          <a href="{{ post.url }}">{{ post.title }}</a>
        </li>
      {% endif %}
    {% endfor %}
  </ul>
</div>