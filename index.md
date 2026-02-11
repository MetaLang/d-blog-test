---
layout: home
title: D Language Blog Archive
---

# All Posts
Explore the finessed D Language Foundation blog history.

<ul>
  {% for post in site.posts %}
    <li>
      {{ post.date | date: "%Y-%m-%d" }} - <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
