---
layout: post
title: "Detection as code, for a six-rule engine"
date: 2026-09-04
tags: [detection-engineering, testing, ci, sigma]
---

Last post, I said the fixtures I wrote for [sigil](https://github.com/noah-fage/sigil)
were no longer the thing I trusted, which is still true. That's not a reason to
delete them though. This is about the test layers around the rules, and the one
thing none of them do.

## The setup

sigil covers six ATT&CK techniques now, not four. Every new rule touches shared
code, the loader, the correlation engine, sometimes the event schema. A change
that fixes rule six can break rule two, and a broken detection rule doesn't
throw an error. It just goes quiet.

So the question isn't whether the new rule works. It's whether I can tell the
other five still do, on every push.

There's three layers now. Unit tests, replay tests, and live Atomic Red Team runs.

The unit tests check each rule against one event it should catch, and a few it
shouldn't, where the false positive cases matter more than the hit. The replay
tests run recorded Sysmon XML through the whole pipeline. Atomic Red Team is the
real technique against real Sysmon in a VM, and it's the only layer that ever
caught Sysmon writing a dash instead of an empty string.

The two new rules, scheduled task creation and ingress tool transfer, now have
all three layers too. I booted the lab VM and ran them against real Atomic Red
Team tests. Both fired. Turned out the VM was still running the old four-rule
set, so the rules had nothing to fire against until I copied the two new files
over myself. I also hit a snag exporting the Sysmon log for replay. wevtutil
wrote UTF-16 on that machine, sigil's replay path reads UTF-8, so the export
parsed into nothing until I forced the encoding.

While I was in there I caught something else too. The process injection rule and
the LSASS rule both fired on their own, against processes I never touched.
Something in the VM's normal background activity looks close enough to both
techniques to trip them, and I haven't tuned for it yet.

That last one is the whole point of this post, honestly. Green CI and a clean
replay run didn't catch it. Only watching the live output did.
