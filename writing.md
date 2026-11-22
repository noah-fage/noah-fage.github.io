---
layout: default
title: Writing
description: "Notes on detection engineering, evaluation, and CTF write-ups."
---

# Writing

Notes on detection engineering and CTF write-ups.

<ul class="post-list">
{% for post in site.posts %}
  <li><span class="date">{{ post.date | date: "%Y-%m-%d" }}</span> <a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
