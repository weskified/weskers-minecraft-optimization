# Wesker's Minecraft Optimization Guide
This repository serves as a personal guide on how I made Minecraft run a lot faster and better! This guide will talk about three (3) different categories of optimizations. Some may be explained really bad, and some may not be, but i hope you have a good enough reading comprehension to understand what I'm actually trying to yap about to you. (if you're really down to reading all of this)

> [!WARNING]
> To be clear, this guide is more of a **personal guide**. Anything written here is what I have done in order to optimize Minecraft, and the performance increase that I have claimed here may be beneficial for your system or not. So results may vary.

> [!CAUTION]
> Before you proceed to reading this, **This is not a complete guide on how to optimize Minecraft Servers**! The main focus of this repository is to optimize the Minecraft Client, just to make it completely clear for you.

## System Specifications
These are my system specs, It's here to show you what kind of machine I am running and doing my benchmarks on to Minecraft. These are to be used as a reference to my benchmarks, and for you to tell how and why im getting THAT much frames in a specific benchmark or something. But while this isn't used for anything much for NOW, it's here just in case when i really get into doing a proper benchmark.

- **CPU**: Intel i3-10100
- **GPU**: AMD RX580 2048SP
  - This graphics card is overclocked to **1450MHz** core clock, with a maximum power draw of **150W**.
- **RAM**: Kingston Beast (16GB) + Kingston Fury (8GB) @ 2666MHz
- **MOBO**: Gigabyte H510M K V2
- **OS**: CachyOS Linux (x64)

## Table of Contents
- [Java Optimizations](#java-optimizations)
  - [Java Distributions](#java-distributions)
  - [Installing a new Java Distribution](#installing-a-new-java-distribution)
    - [Installing via Launcher](#installing-via-launcher)
    - [Installing Manually](#installing-manually)
    - [Java Arguments](#java-arguments)
    - [Java Arguments Explanation](#java-arguments-explanation)
      - [Sources](#sources)
- [System Optimizations](#system-optimizations)
  - [Huge Pages](#huge-pages)
    - [There are two types of Huge Pages.](#there-are-two-types-of-huge-pages)
    - [What's the benefit for enabling this on Minecraft?](#whats-the-benefit-for-enabling-this-on-minecraft)
  - [Turning on Huge Pages in your OS](#turning-on-huge-pages-in-your-os)
    - [Windows](#windows)
    - [Linux (temporary)](#linux-temporary)
    - [Linux (permanent, service based)](#linux-permanent-service-based)
  - [Electron Applications](#electron-applications)
- [Other Optimizations](#other-optimizations)

## Java Optimizations
Tweaking the JRE (Java Runtime Environment) is pretty much the first thing that *some* people has in mind when having a new fresh install of Minecraft. as if you don't, you'll pretty much suffer from performance degradation which most people probbably experience until they search up "How to make minecraft run faster" on youtube or something. Which in some days, i have seen people like that... don't be like them.

### Java Distributions
> [!NOTE]
> "**Java Distributions**" as in OpenJDK, Adoptium, Azul Zulu, GraalVM, etc. Like those completely different smart guy companies that makes completely different Java versions that some people talk about and run minecraft on. like me lol

First of all, I'm gonna be real here. The provided mojang JRE fucking sucks (apparently), so in order to get some performance increase, changing the Java Distribution that you're using can allow Minecraft to run better. and the best two distributions that I can recommend you is either Adoptium (for stability) and Azul Zulu (for performance). But both works well, but for myself i personally use Azul Zulu because why not?

### Installing a new Java Distribution
In this guide, we will be installing the **latest LTS (Long-Term Support) release**, being Java 25. As it offers a lot of optimizations since Java 21. (and runs a lot better)

#### Installing via Launcher
I think pretty much all of us uses a launcher to run Minecraft. at least for the Java edition, since that's what you're here for. And the most popular one that people know is Prism Launcher and Modrinth. But for this guide, we will be using Prism Launcher instead, as it is the best universal launcher for Minecraft I guess.

Anyways, The installation should be pretty simple, go to **Settings**.
<img width="1244" height="1128" alt="image" src="https://github.com/user-attachments/assets/5f9a02c4-cb1a-4639-a8bd-1865567a1698" />

Select **Java**, go to Installations and Download either Adoptium or Azul Zulu. Then press **Download**.
<img width="2256" height="1624" alt="image" src="https://github.com/user-attachments/assets/48d8eece-63de-4ca4-b264-2b81c85ef240" />

aaaaaaaaaaaaaaaand you're pretty much set from here. Just make sure that the version that you're using is the correct one that you downloaded, and make sure to check the two skips shown there. (for Prism Launcher)

> [!NOTE]
> For Modrinth Launcher. The launcher automatically downloads from Azul Zulu, and you cannot change this. (It doesn't matter anyways)

#### Installing Manually
So you want to give yourself a hard time just to install a JRE for Minecraft? cool! This section exists anyways for people like you. And for this one, we will also be installing **Java 25** for this, as its faster, runs better, and pretty much the most latest LTS release that you can run Minecraft on. (at least at the time of writing, January 24, 2026)

So to do this as i mentioned, We will be installing the latest version of Java, which would be **Java 25**. You can install [Adoptium](https://adoptium.net/temurin/releases?version=25&os=any&arch=any) or [Azul Zulu](https://www.azul.com/downloads/?version=java-25-lts&package=jdk#zulu), then download the JRE compatible with your system. (These links to the Java version so you don't have to, just make sure to select the actual operating system that you're using. you should NOT get this process wrong)

Once you downloaded the JRE in your computer, it should be at the **Downloads** folder. Select the ZIP file that downloaded which has the JRE inside, and extract it to somewhere neat. (Like `C:/java` or something. up to you) And then copy the `javaw.exe` (windows) or `java` (linux) path inside the `bin` folder to your launcher.

It should look something like this:
<img width="1080" height="610" alt="image" src="https://github.com/user-attachments/assets/b11bfad2-3563-4c16-adeb-1a9b70049224" />
(This was done by automatic download from the launcher, but you know what to do anyways.. right?)

#### Java Arguments
These are the Java Arguments that I **PERSONALLY** use for Minecraft, it's not the best.. but it's enough to make minecraft run a lot better and faster!
```
-XX:+UseZGC -XX:+UseCompactObjectHeaders -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:+PerfDisableSharedMem -XX:+UseTransparentHugePages -XX:-DontCompileHugeMethods -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1
```

> [!NOTE]
> The argument makes the JRE use **Transparent Huge Pages** in linux. For more information about that, Check out the [Huge Pages](#huge-pages) part in the [System Optimizations](#system-optimizations) section.
>
> Having the flag stay on the args list does nothing, and just leaves an error on the minecraft logs. That's it... (its safe to keep it there unless you want to turn on Huge Pages)

> [!WARNING]
> For windows, replace `-XX:+UseTransparentHugePages` with `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`! In linux, I do not recommend using the two together.

#### Java Arguments Explanation
- `-XX:+UseZGC`: Enables the Z Garbage Collector, a low-latency GC that does most of its work concurrently with your application threads. This means Minecraft won't freeze as much during garbage collection because ZGC doesn't need to pause everything to clean up memory.

- `-XX:+UseCompactObjectHeaders`: A newer feature in Java 25 that reduces the memory overhead of object headers in the heap, resulting in lower memory usage and better performance overall.
  
- ⚠️ `-XX:+DisableExplicitGC`: Tells the JVM to ignore manual garbage collection calls (like `System.gc()`) that might be in Minecraft mods or code. This prevents poorly-timed GC from messing with ZGC's optimized collection schedule. The downside is that some code relying on explicit GC might not reclaim memory as expected, but this is rarely an issue unless you're running on extremely limited memory.
  
- `-XX:+AlwaysPreTouch`: Forces the JVM to actually touch and initialize all allocated heap memory during startup, rather than doing it lazily as needed. This makes startup slower but can improve runtime performance by avoiding page faults later.
  
- `-XX:+PerfDisableSharedMem`: Disables the JVM from writing performance statistics to `/tmp/hsperfdata_*` shared memory files. This reduces disk I/O operations that can cause microstutters.
  
- `-XX:+UseTransparentHugePages` or `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`: Enables the use of larger memory pages (2MB instead of the default 4KB). This reduces the overhead of managing the page table since the JVM needs to track fewer, larger pages instead of tons of tiny ones.
  
- `-XX:-DontCompileHugeMethods`: Allows the JIT compiler to compile very large methods that it would normally skip. By default, the JVM avoids compiling huge methods because they take a long time to compile, but this flag forces it to compile them anyway for potential performance gains.
  
- `-XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M`: These control the code cache where the JVM stores compiled native code. Java starts as bytecode, then the JIT compiler converts frequently-used methods into faster native machine code. These flags allocate 400MB total for the code cache (vs the default ~240MB) and divide it into segments for different types of compiled code.
  
- `-XX:NmethodSweepActivity=1`: Controls how aggressively the JVM cleans up old/unused compiled code from the code cache. A value of 1 is very aggressive, meaning it'll more readily remove old compiled methods to make room for new ones.

##### Sources
- https://exa.y2k.diy/garden/jvm-args/ (because of [#1](https://github.com/weskified/weskers-minecraft-optimization/issues/1))
- People from [BruceTheMoose's discord server](https://discord.gg/u3NgjC23Qm) for some advice over these :3

## System Optimizations
Not just having a new fresh JRE and flags can save you for not getting Minecraft to run faster. There are a lot more things to note down in mind in order to make Minecraft run a lot better. But this section mainly focuses more on getting better frames and not making processing a lot faster.

### Huge Pages
Enabling this in your system allows memory intensive applications like Chrome, Minecraft, etc. from running a lot faster! This works because of how your computer manages memory.

Normally, your computer breaks up RAM into tiny chunks like "pages". Like Minecraft Chunks that stores a part of your world saving about 16x16 blocks. In a computer way, it would be storing about 4KB of memory for each pages. Which makes your computer track all of the tiny little pages, which when we want to allocate memory to it, It's like finding the very smallest grain of rice in a huge pile of them. (very hard for the system, as to what im trying to say here)

But with Huge pages, basically it converts the Minecraft chunk of 16x16 blocks into 32x32, or even more (in our case, allocating about 2MB of pages), which makes:
- Your processor no longer tracks tiny pages (4KB), and tracks huge ones (2MB) instead. (That's why it's called **Huge Pages**)
- Your processor has a special cache (mainly what the smart guys call TLB) that remembers where these pages are. And with **Huge Pages**, each entries covers way more memory, so it finds that a lot more often.
  - Think of it like you in a Minecraft world trying to find the stronghold with an ender eye. And with Ninjabrain (in a systematic-ish sense, Huge Pages), this makes finding the stronghold a lot faster with almost precise coordinates.
- Applications that use a lot of ram (like Minecraft), spends less time managing memory and more time to actually do other stuff instead.

#### There are two types of Huge Pages.
For **Windows**, well.. they're called Huge Pages.\
For **Linux**, they're called the same, but the better version of it is called Transparent Huge Pages (or THP).

#### What's the benefit for enabling this on Minecraft?
For now... in MY testing, It pretty much made FPS stabilize a LOT faster. from a few minutes with Huge Pages turned off, with about under 10 seconds for FPS to stabilize. Which is pretty neat! But i would have to look at this more.

Could also probably help with garbage collection too, but I have not tried or observed that yet.

### Turning on Huge Pages in your OS
These processes aren't really that scary, believe me. as this is actually a lot more beneficial turning this on rather than having it disabled. And with that, we will go straight to windows first, as Winslop apparently has it disabled for some unknown reason.

#### Windows
- Press `Win + R`, type `secpol.msc`, and press `Enter`.
- Go to `Security Settings` > `Local Policies` > `User Rights Assignment`.
- Inside that, find the **Lock pages in memory** policy and then double click that policy.
- Click **Add User or Group**, type your username (the name of the windows user account that you're using currently), then click **OK**.
- Click **Apply** then restart your computer.

Here's a screenshot of me doing this exact thing when I was still using windows:
<img width="1898" height="810" alt="image" src="https://github.com/user-attachments/assets/eff764ad-8d30-4e2c-9851-9c1b193411a9" />

Huge Pages should be setup, to verify that, check logs for any huge pages related errors. And if that shows up, you did something wrong.

#### Linux (temporary)
```
echo advise | sudo tee /sys/kernel/mm/transparent_hugepage/shmem_enabled
```

> [!NOTE]
> To turn this off, just replace `echo advise` to `echo never` once you're done playing minecraft. Or just reboot your entire system if you feel like it. :3

#### Linux (permanent, service based)
```
sudo nano /etc/systemd/system/thp-shmem.service
```

Paste this inside the systemd service file:
```
[Unit]
Description=Enable THP for shared memory
After=sysinit.target

[Service]
Type=oneshot
ExecStart=/bin/sh -c 'echo advise > /sys/kernel/mm/transparent_hugepage/shmem_enabled'
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

Enable it with:
```
sudo systemctl daemon-reload
sudo systemctl enable thp-shmem
sudo systemctl start thp-shmem
```

### Electron Applications
Oh boy! ...oh no!\
Electron applications is the most biggest graphics munchers that can either tank down your FPS by a little.. to a LOT. And this section covers a solution to fix that performance decrease, with a few requirements in mind in order to actually have this work as intended. (in my system it does, ..does yours do?)

## Other Optimizations
This section focuses on other stuff.
