---
layout: default
title: CTF write-ups
description: "CTF write-ups with the commands I ran and why, including the dead ends."
---

# CTF write-ups

These CTFs are done on CyLab Security Academy.

<!-- Newest write-up goes at the TOP. Give each h2 an id for jump links. -->

<h2 class="ctf-title" id="binary-digits">Binary Digits <span class="ctf-meta">(Easy) - 10 minutes</span></h2>

* The challenge gave me a file called `digits.bin`. I ran `strings digits.bin | grep -i flag` and found nothing. Then `file` said "ASCII text" with one very long line, so it wasn't a broken file, just text. I ran `head -c 200 digits.bin` and it was only 0s and 1s, which fits the title. A byte is 8 binary digits, so someone had written a file out as text, 8 characters per byte. I converted the first 4 bytes by hand to `FF D8 FF E0`. I remembered that from the corrupted file challenge as the start of a JPEG, so the text was an image. I first tried `xxd -r -p digits.bin > out.jpg`, and it made garbage because `-p` expects hex, not binary. I fixed it with a short Python script, which I got help writing, that turns each group of 8 digits into one byte and writes them to `out.jpg`. `file out.jpg` said JPEG image data, and I opened it with `explorer.exe out.jpg` and found the flag in the picture.

<h2 class="ctf-title" id="corrupted-file">Corrupted file <span class="ctf-meta">(Easy) - 15 minutes</span></h2>

* The challenge gave me a file that wouldn't open. I ran `file` on it and it just said `data`, which I learned means the program couldn't recognize the file type. Every file starts with a few bytes that tell programs what type it is, and this one's start was broken. I ran `xxd file | head` to look at the first bytes. The text column on the right spelled out `JFIF`, which I looked up and found is the marker inside JPEG files, so the file was probably a JPEG. I searched for JPEG magic bytes and found that a JPEG should start with `FF D8 FF E0`. My file started with `5c 78 FF E0`, so the first two bytes were wrong. I made a backup with `cp file file.bak`, then used `hexedit` to overwrite those two bytes with `FF D8` and saved with Ctrl+X. Running `file` again said JPEG image data. I copied it to `fixed.jpg`, opened it with `explorer.exe fixed.jpg`, and found the flag in the picture.

<h2 class="ctf-title" id="hidden-in-plainsight">Hidden in plainsight <span class="ctf-meta">(Easy) - 5 minutes</span></h2>

* The challenge gave me an image, so I searched for how to discover a hidden payload in a JPG on Linux. The results pointed to `exiftool` for the metadata, so I ran that first. The Comment field was a string of random characters, so I decoded it with `base64 -d` and got `steghide:` followed by another string ending in "=". I tried to decode the whole thing at once and got "invalid input", since the `steghide:` part is plain text and not Base64. So I decoded only the part after the colon, and it turned out to be a password. I searched what steghide is and learned that it hides files inside images and needs a passphrase to get them back out, then searched how to extract with it. I installed it with `sudo apt install steghide`, ran `steghide extract -sf img.jpg`, typed the password when it asked, and it wrote out `flag.txt`. I ran `cat flag.txt` and found the flag.

<h2 class="ctf-title" id="riddle-registry">Riddle Registry <span class="ctf-meta">(Easy) - 3 minutes</span></h2>

* The description said the flag should be uncovered within "metadata". I hadn't done CTFs before, but I knew they're kind of like cryptic crossword puzzles, in the sense that the instructions usually give obvious clues about how to find the answer. So I deduced that I should look up how to view PDF metadata on Linux, which led me to `exiftool`. Then I noticed that the Author line was the obvious abnormality, since it was a string of random characters ending with "=", so that was clearly a string in Base64. So I looked up how to decode Base64. Finally I ran `echo '<the string ending with =>' | base64 -d` and found the flag.
