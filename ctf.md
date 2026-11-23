---
layout: default
title: CTF write-ups
description: "CTF write-ups with the commands I ran and why, including the dead ends."
---

# CTF write-ups

These CTFs are done on CyLab Security Academy.

<h2 class="ctf-title">Riddle Registry <span class="ctf-meta">(Easy) - 3 minutes</span></h2>

* The description said the flag should be uncovered within "metadata". I hadn't done CTFs before, but I knew they're kind of like cryptic crossword puzzles, in the sense that the instructions usually give obvious clues about how to find the answer. So I deduced that I should look up how to view PDF metadata on Linux, which led me to `exiftool`. Then I noticed that the Author line was the obvious abnormality, since it was a string of random characters ending with "=", so that was clearly a string in Base64. So I looked up how to decode Base64. Finally I ran `echo '<the string ending with =>' | base64 -d` and found the flag.

<h2 class="ctf-title">Hidden in plainsight <span class="ctf-meta">(Easy) - 5 minutes</span></h2>

* The challenge gave me an image, so I searched for how to discover a hidden payload in a JPG on Linux. The results pointed to `exiftool` for the metadata, so I ran that first. The Comment field was a string of random characters, so I decoded it with `base64 -d` and got `steghide:` followed by another string ending in "=". I tried to decode the whole thing at once and got "invalid input", since the `steghide:` part is plain text and not Base64. So I decoded only the part after the colon, and it turned out to be a password. I searched what steghide is and learned that it hides files inside images and needs a passphrase to get them back out, then searched how to extract with it. I installed it with `sudo apt install steghide`, ran `steghide extract -sf img.jpg`, typed the password when it asked, and it wrote out `flag.txt`. I ran `cat flag.txt` and found the flag.

