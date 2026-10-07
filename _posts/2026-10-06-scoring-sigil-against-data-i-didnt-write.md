---
layout: post
title: "Scoring sigil against data I didn't write"
date: 2026-10-06
tags: [detection-engineering, evaluation, sigma, false-positives]
---

Every test in [sigil](https://github.com/noah-fage/sigil) passes. Every rule
fires in my lab. That tells me less than it sounds like, since I wrote the
rules, the tests, and the attacks I ran against them. So I went and found data
I didn't write.

## What I scored it against

EVTX-ATTACK-SAMPLES is a public repo of recorded Windows event logs from real
attack techniques. 278 files, 3,241 Sysmon events. I sorted the files by
technique from their folder and file names, and I wrote the sorting rule down
before I looked at a single result. A file only counts if it has the kind of
Sysmon event that technique's rule actually reads. A lot of the samples are
Security log events, which sigil never sees, so those don't count as misses.

I split the files in half. One half to tune on, one half I wouldn't touch.

For false alarms I needed normal activity, so I ran Sysmon on my own PC with
sigil's config for about 27 hours and just used the computer. Coding, browsing,
class stuff. 14,235 events.

## It missed most of it

5 of 34.

| Technique | Before | After |
|---|---|---|
| LSASS access | 0 of 3 | 3 of 3 |
| Process injection | 0 of 4 | 4 of 4 |
| Run key persistence | 0 of 2 | 2 of 2 |
| Scheduled task | 3 of 4 | 3 of 4 |
| Ingress tool transfer | 0 of 1 | 1 of 1 |
| LOLBin execution | 2 of 20 | 8 of 20 |
| Total | 5 of 34 | 21 of 34 |

On the half I held out it went from 5 of 21 to 11 of 21.

## Two of the misses were the StartModule bug again

The LSASS rule checked the access mask as text. `0x1fffff`. The samples write
it as `0x001fffff`, padded with zeros, so the rule never matched any of them.
The injection rule wanted a dash for a thread with no module, since that's what
Sysmon 15.21 writes in my VM. Older Sysmon writes nothing at all.

Both rules were right on my machine and wrong on everyone else's. Same lesson
as the first post, I tuned against what one Sysmon version emits and not
against what Sysmon emits. Fixing just those two took LSASS, injection and the
run key rule from zero to everything.

I'll be honest that I found both by looking at all the samples, so the
held-out gain from those two isn't clean. The coverage changes after that came
only from the tuning half.

## The rule that's still weak

The LOLBin rule. 2 of 20 at first, 8 of 20 now, and on the held-out half only
4 of 14. I added the rundll32 URL handler tricks, mshta running out of staging
folders, and a couple of others, but real attackers use a lot more of these
than my lab tests ever touched. That one has more work in it.

## False alarms

12 on a normal day with the original rules. All twelve came from the process
injection rule. Ten of them were Windows itself. The Desktop Window Manager
creating a thread in csrss.exe, which it does constantly and which is fine. I
excluded that exact pair by full path, so something named dwm.exe sitting in a
random folder still fires (there's a test for it).

The other two are specific to my own machine, so I left them.

That takes it from 12 to 2. The rules I widened raised nothing at all, and
there were 4,519 LSASS access events in that day. I did find the dwm.exe
problem and measure the fix on the same day of data though, so the 12 to 2 is
optimistic. A second day is the real test.

## Speed

About 70,000 events a second with the rules and the correlation running, on my
PC. The same PC produced about one event every seven seconds all day. Speed
isn't the bottleneck.

## What this doesn't show

Small samples, one of the techniques has a single usable file. One machine, one
day. Lab recordings from older Sysmon versions. Rules I wrote and then tuned
myself. It tells me where the rules break, it isn't a benchmark.

I still want a second day of baseline, and the LOLBin rule needs a proper pass.
I'll write it up when I get there. The full numbers, the labels and the
scripts are in the repo under
[evaluation](https://github.com/noah-fage/sigil/tree/main/evaluation).


