---
layout: default
title: CTF
description: "CTF and forensics write-ups, including the dead ends."
---

# CTF write-ups

Forensics and blue team challenges, one write-up each. I include the dead ends,
since that's where most of the learning happens. I'm starting with picoCTF and
TryHackMe forensics.

{% assign ctf_posts = site.tags.ctf %}
<ul class="post-list">
{% for post in ctf_posts %}
  <li><span class="date">{{ post.date | date: "%Y-%m-%d" }}</span> <a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
