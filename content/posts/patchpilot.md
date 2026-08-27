---
title: "PatchPilot: Building a Patch Manager Because I Wanted the Dashboard"
date: 2026-08-26T09:00:00-06:00
draft: true
tags: ["ubuntu", "linux", "automation", "systemd", "homelab"]
categories: ["projects"]
summary: "unattended-upgrades works fine. It just never tells you anything. So I built something that does."
ShowToc: true
---

## The problem

I run a fair number of Ubuntu hosts at home. Not a datacentre, but enough that
"SSH into each one and run apt" stopped being a reasonable Saturday activity a
while ago.

The standard answer is `unattended-upgrades`, and it works. It installs security
updates, it does not ask questions, and it mostly stays out of the way. My problem
with it was never reliability. It was visibility.

I could not answer basic questions without logging into individual machines:

- Which hosts have pending updates right now?
- Which ones need a reboot and have been quietly needing one for three weeks?
- Did the patch run last night actually succeed everywhere?
- When did this specific host last get touched?

Every one of those is answerable. None of them is answerable *at a glance*, which
in practice means I did not answer them.

## What I wanted

A short list:

1. A single page showing every host, its pending update count, and its reboot state.
2. Scheduled patch runs I could see the results of without reading mail.
3. History, so I could tell when something started going wrong rather than just
   that it currently is.
4. No agent install beyond what a fresh Ubuntu box already has.

That last one mattered more than the rest. Anything requiring a heavy agent was
going to rot the first time I rebuilt a host and forgot to reinstall it.

## Architecture

<!-- TODO: describe the actual shape here - collector, store, web layer -->

The rough shape:

- **Collection** runs on each host via a systemd timer, gathers state, and reports back.
- **Storage** keeps current state plus enough history to be useful.
- **Web dashboard** renders it.

### Why systemd timers instead of cron

<!-- TODO: expand - journald logging, dependency handling, timer accuracy -->

Cron would have worked. I went with systemd timers for the logging, mostly. When a
run fails at 3am, `journalctl -u patchpilot` tells me what happened without me
having to have set up mail forwarding correctly six months earlier.

## What I would do differently

<!-- TODO: honest retrospective - this section is what makes the post worth reading -->

## Where it is

<!-- TODO: GitHub link once published -->

---

*If you are running something similar and solved a piece of this differently, I would
genuinely like to hear about it.*
