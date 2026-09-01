---
layout: post
title: "A detection that passed every test and never fired"
date: 2026-08-31
tags: [detection-engineering, sysmon, sigma]
---

I've been building [sigil](https://github.com/noah-fage/sigil). It's a detection
engine that evaluates Sigma rules against live Windows Sysmon telemetry and it
correlates the hits into attack-chain alerts. One of the four techniques it
covers is process injection through `CreateRemoteThread` (MITRE ATT&CK T1055). The
rule for it had unit tests, those tests passed, and against a real attack it
caught nothing.

## What the rule was supposed to do

When a process creates a thread inside another process, Sysmon logs Event ID 8.
And if that thread starts at an address that doesn't map to any module on disk,
that's a strong signal of injected code. The attacker wrote shellcode into the
target and started a thread pointing at it. Sysmon represents "no backing
module" in the `StartModule` field.

My rule looked for exactly that. Event ID 8 where `StartModule` is empty.

## What Sysmon actually writes

I had assumed "empty" meant an empty string, so the rule and its test fixtures
both used `StartModule: ''`. Nothing fired when I ran the Atomic Red Team test for
T1055 and watched the engine. The correlation view stayed quiet and the unit
tests were still green.

I pulled the raw event out of the Windows event log and looked at the XML. The
field was there and it wasn't an empty string, but a single dash:

```xml
<Data Name="StartModule">-</Data>
```

Sysmon writes a literal `-` for an unresolved module, not `""`. My rule was
checking for a value that real telemetry never produces, so it couldn't match
anything. The fix was one character in the rule and one in the fixture.

## Why the tests didn't catch it

The tests weren't wrong about the logic, they were wrong about the input. I'd
written the fixtures from my mental model of Sysmon output, and the rule was
built to match those same fixtures, so the two agreed with each other and told
me nothing about reality. A synthetic fixture can only encode the assumptions
you already have.

The only thing that surfaced the bug was running the actual technique and
diffing what I expected against what the sensor emitted.

## The other thing that happened

While debugging this I also couldn't get the injection test to complete. The
Atomic Red Team test injects into `werfault.exe` by default, and Windows
Defender's behavioral engine kept killing it mid-injection even with real-time
protection turned off. Tamper Protection seems to re-enable protection quietly
on the image I was using. I retargeted the test at `notepad.exe` with
`-InputArgs`, which exercises the same technique without tripping that response
and got a clean run.

## What I learned

Unit tests on a detection rule tell you the rule does what you think it does.
They don't tell you whether what you think is true. For anything that parses
telemetry, the ground truth is the telemetry, and the way to get it is to run
the real technique against a real sensor and read the output byte for byte.

sigil now validates every rule against a live Atomic Red Team run in an isolated
VM, not just against fixtures I wrote. The fixtures are still there for fast
feedback. They are just no longer the thing I trust.
