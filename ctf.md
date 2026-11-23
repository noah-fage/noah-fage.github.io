---
layout: default
title: CTF write-ups
description: "CTF write-ups with the commands I ran and why, including the dead ends."
---

# CTF write-ups

These CTFs are done on CyLab Security Academy.

<h2 class="ctf-title">Riddle Registry <span class="ctf-meta">(Easy) - 3 minutes</span></h2>

* The description said the flag should be uncovered within "metadata". I hadn't done CTFs before, but I knew they're kind of like cryptic crossword puzzles, in the sense that the instructions usually give obvious clues about how to find the answer. So I deduced that I should look up how to view PDF metadata on Linux, which led me to `exiftool`. Then I noticed that the Author line was the obvious abnormality, since it was a string of random characters ending with "=", so that was clearly a string in Base64. So I looked up how to decode Base64. Finally I ran `echo '<the string ending with =>' | base64 -d` and found the flag.
