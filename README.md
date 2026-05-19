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

## Day 2 — May 17, 2026

**What I did:**
- Just looked around the files and nothing much

## Day 3 & 4 — May 18-19, 2026 
### The Great Build, Hardware Warfare, and Separation of Concerns

#### 🛠️ What I Did
* **Compiled Android 14 from Source:** Successfully built the entire OS (150GB+ workspace).
* **Fixed Blueprint Targets:** Diagnosed a boot failure caused by building a GSI (`aosp_x86_64`) instead of an emulator target. Switched the blueprint to `sdk_phone_x86_64-eng`.
* **Fought Physical Hardware Limits (2025 Lenovo LOQ):**
  * *OOM Prevention:* Forged a tactical 32GB swap file on the fly to stop Soong/Ninja memory crashes.
  * *0-Byte File Sweep:* Used `find` commands to hunt and destroy corrupted `.sdump` and `.lsdump` JSON files that were failing the build after the partition hit 100% capacity.
  * *Storage Rerouting:* Rerouted the 12GB `userdata-qemu.img` to my main root partition when the `aosp-out` drive ran out of space.
* **Booted the OS:** Successfully launched my custom AOSP build in the QEMU emulator.
* **Explored the Framework:** Navigated the AOSP framework in VS Code (after learning to disable Java Language Servers to prevent massive system freezes).

#### 🧠 What I Learned
* **The Build System is Smart:** Ninja caches C++ objects. Switching targets doesn't mean starting a 5-hour build over from scratch; it just means linking new blueprints.
* **Hardware Management is Crucial:** Compiling an OS requires strict storage and memory management. You have to monitor partitions, manage swap files, and understand what the Linux kernel is doing under heavy loads.
* **Separation of Concerns (The Grand Architecture):** Enterprise software strictly separates the UI from the logic engine.
  * **The Canvas (XML):** Files like `display_settings.xml` just draw the buttons on the screen. They are dumb paint.
  * **The Brain (Java Controllers):** Files like `DarkUIPreferenceController.java` listen for the XML button clicks.
  * **The Boss (System Services):** The Controller passes the message to massive backend services (like `UiModeManager`), which execute core commands (like `setNightMode`) to actually talk to the hardware.
