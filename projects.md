---
layout: default
title: Projects
---

# Projects

## sigil

A real-time detection engine. It evaluates Sigma rules against live Windows
Sysmon telemetry, correlates related events across processes into scored alerts
mapped to MITRE ATT&CK, and exports ATT&CK Navigator layers. It covers six
techniques. Four of them I validated in an isolated lab against live Atomic Red
Team attack simulations.

![sigil flagging an LSASS credential-dumping attempt in the lab, tagged to ATT&CK T1003.001, next to a running Atomic Red Team test](/assets/img/sigil-lsass-detection.png)

Since then I added a CI pipeline that runs the tests on every push, replay tests
that push recorded telemetry through the whole engine and check what it flags,
and two more rules. Still on the list: validating those two new rules against
live attacks, and Linux coverage alongside Windows.

[github.com/noah-fage/sigil](https://github.com/noah-fage/sigil)

## Sentinel

A SOC analytics dashboard built with FastAPI and React. It ingests logs and
triages intrusion patterns, privilege escalation, and data exfiltration,
scoring each finding with cited evidence, then projects an attacker's likely
next moves and countermeasures.

[sentinel-anomaly.vercel.app](https://sentinel-anomaly.vercel.app)
