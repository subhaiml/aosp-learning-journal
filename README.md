# AOSP Learning Journal

**Started:** May 16, 2026  
**Goal:** Android Platform Engineering  
**Method:** Building Android 14 from source on Ubuntu 26.04

---

## Why I'm Doing This

I've been interested in Android internals since class 8.
Flashed custom ROMs, debugged bootloops, bricked and fixed phones.
Now I'm going deeper — not just flashing ROMs but understanding
and building the actual Android OS layer by layer.

---

## What Android Platform Engineering Actually Is

Android has layers:
1. Linux Kernel — talks to hardware
2. HAL (Hardware Abstraction Layer) — bridges kernel and Android
3. Android Runtime (ART) — runs Java apps
4. Framework (Java) — system services, Settings, ActivityManager
5. Apps — what users see

Platform engineers work on layers 1-4.
Not the apps. The actual operating system.

When I modified ROMs in class 8, I was replacing these layers
without knowing what they were. Now I'll understand each one.

---

## Day 1 — May 16, 2026

**What I did:**
- Set up Ubuntu 26.04 with all AOSP build dependencies
- Started AOSP source sync (android-14.0.0_r1, ~80GB)
- Read Android architecture overview

**What I learned:**
AOSP is 800+ git repositories managed together by Google's repo tool.
Android 14 is the right version to learn — stable, well-documented.
The sync takes 4-8 hours because it downloads 80GB of source code.
