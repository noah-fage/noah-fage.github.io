---
layout: default
---

CS student at McGill, focused on detection engineering. I build tooling that
catches attacker behavior in system logs, test it against real attack
simulations in a home lab, and dig into how detection holds up at scale. My main
project, [sigil](/projects/), is a real-time detection engine I built from
scratch and validated against live attacks. I write here about what I learn
building it.

Outside of that: weightlifting and playing keys in
[Blue Moon Brothers](https://open.spotify.com/search/Blue%20Moon%20Brothers).

<!-- swap the Spotify search link for the direct artist URL once you have it. add a third interest here if you want one. -->

Recent writing:

<ul class="post-list">
{% for post in site.posts limit:5 %}
  <li><span class="date">{{ post.date | date: "%Y-%m-%d" }}</span> <a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
