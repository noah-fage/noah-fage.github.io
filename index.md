---
layout: default
---

CS student at McGill. I'm into detection engineering, home labs, and writing
about what I learn. Still figuring out most of it.

Outside of that: weightlifting, playing keys in
[Blue Moon Brothers](https://open.spotify.com/search/Blue%20Moon%20Brothers),
and reading more security writeups than I can keep up with.

<!-- swap the Spotify link for the direct artist URL once you have it. change the third interest to whatever's real. -->

Recent writing:

<ul class="post-list">
{% for post in site.posts limit:5 %}
  <li><span class="date">{{ post.date | date: "%Y-%m-%d" }}</span> <a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
