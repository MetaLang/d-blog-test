---
layout: default
title: D Language Blog Archive
---
<div style="max-width: 1200px; margin: 0 auto; padding: 40px 20px;">
  <h1>Archive</h1>
  <p>A comprehensive history of the D Programming Language development blog.</p>

  <ul style="list-style: none; padding: 0;">
    {% for post in site.posts %}
      <li style="margin-bottom: 15px;">
        <span style="color: #888; font-size: 0.9em;">{{ post.date | date: "%Y-%m-%d" }}</span> —
        <a href="{{ post.url | relative_url }}" style="color: #b03931; font-weight: bold; text-decoration: none;">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
</div>
