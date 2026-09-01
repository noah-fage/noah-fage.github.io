---
layout: post
title: "Detection as code, for a six-rule engine"
date: 2026-09-04
tags: [detection-engineering, testing, ci, sigma]
---

<!--
DRAFT. Final voice pass before publishing. Check every claim against the sigil
repo and the CI run. A screenshot of the green CI badge or the Actions run would
work well here.
-->

Last post, I said the fixtures I wrote for [sigil](https://github.com/noah-fage/sigil)
were no longer the thing I trusted, which is still true. It's also not a reason to
delete them. This is about the test layers around the rules as well as the one
thing none of them do.

## The setup

sigil now covers six ATT&CK techniques instead of four. Every new rule touches
shared code which includes the loader, the correlation engine, and sometimes the
event schema. A change that fixes rule six for example can break rule two, and a
broken detection rule doesn't throw an error but instead just goes quiet.

So the question is not whether the new rule works, but whether I can tell that the
other five still do on every push.

Now there's also three layers. Unit tests, replay tests, and live Atomic Red Team
runs.

The unit tests check each rule against one event it should catch (and a few it
shouldn't) where the false positive cases matter more than the hit. For the
replay tests, recorded Sysmon XML runs through the whole pipeline. And Atomic Red
Team, the real technique against real Sysmon in a VM. This is the only layer that
caught Sysmon writing a dash instead of an empty string.

The two new rules, scheduled task creation and ingress tool transfer, only have
the first two layers so far. I still need to boot the lab VM and run them against
real attacks before I call them done. That is the next step, and I will write it
up when I get there.

Finally, continuous integration (CI) being green means I haven't regressed. It
doesn't mean I'm right. Those are different, and it took me a while to stop
treating them as the same thing.
