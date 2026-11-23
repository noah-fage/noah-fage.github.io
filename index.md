---
layout: default
---

Hey there! I'm a CS student at McGill, focused on detection engineering. I build
tooling that catches attacker behavior in system logs, test it against real
attack simulations in a home lab, and dig into how detection holds up at scale.
My main project, [Sigil](/projects/), is a real-time detection engine I built
from scratch and validated against live attacks. I write here about what I learn
building stuff, my CTFs, writings, and more! Outside of all this I love
weightlifting, playing keys in my band the
[Blue Moon Brothers](https://open.spotify.com/search/Blue%20Moon%20Brothers),
and playing chess.

I'm looking for a Summer 2027 cybersecurity internship.

<p class="find-me">
<a href="/assets/Noah_Fage_Resume.pdf">Resume (PDF)</a>
<a href="https://github.com/noah-fage">GitHub</a>
<a href="https://www.linkedin.com/in/noah-fage-139885209">LinkedIn</a>
<a href="mailto:noah.fage@mail.mcgill.ca">Email</a>
</p>

## Certifications

- CompTIA Security+ (SY0-701), July 2026, valid through July 2029.
  [Certificate (PDF)](/assets/comptia-security-plus.pdf)
- CompTIA Network+ (N10-009), October 2026, valid through October 2029.
  [Certificate (PDF)](/assets/comptia-network-plus.pdf)

Both can be checked at [verify.comptia.org](https://verify.comptia.org).

## Projects

- [Sigil](/projects/#sigil), a real-time detection engine for Windows Sysmon
  telemetry.
- [Sentinel](/projects/#sentinel), a SOC dashboard built with FastAPI and React.
- [IoT attack-injection testbed](/projects/#iot), research at Concordia.
- [Rover video pipeline](/projects/#rover), McGill Robotics.

## Recent writing

<ul class="post-list">
  <li><a href="{{ '/ctf/' | relative_url }}">CTF write-ups</a></li>
{% for post in site.posts limit:5 %}
  <li><span class="date">{{ post.date | date: "%Y-%m-%d" }}</span> <a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
