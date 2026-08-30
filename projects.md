---
layout: default
title: Projects
---

# Projects

## sigil

A real-time detection engine. It evaluates Sigma rules against live Windows
Sysmon telemetry, correlates related events across processes into scored alerts
mapped to MITRE ATT&CK, and exports ATT&CK Navigator layers. I validated it in
an isolated lab against live Atomic Red Team attack simulations across four
techniques.

![sigil flagging an LSASS credential-dumping attempt in the lab, tagged to ATT&CK T1003.001, next to a running Atomic Red Team test](/assets/img/sigil-lsass-detection.png)

Next: a CI pipeline that replays recorded telemetry and checks every rule still
fires on each commit, a wider ruleset, and Linux coverage alongside Windows.

[github.com/noah-fage/sigil](https://github.com/noah-fage/sigil)

## Sentinel

A SOC analytics dashboard built with FastAPI and React. It ingests logs and
triages intrusion patterns, privilege escalation, and data exfiltration,
scoring each finding with cited evidence, then projects an attacker's likely
next moves and countermeasures.

[sentinel-anomaly.vercel.app](https://sentinel-anomaly.vercel.app)
