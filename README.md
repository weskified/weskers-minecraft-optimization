# Wesker's Minecraft Optimization Guide
This is a personal guide made by me, to show to other people on how I made Minecraft run a lot faster and better!\
This guide will talk about quite a lot of things, ranging from different categories of Optimizations for a Client and Server configuration! Some explanations may be.. bad, some might not. So a bit of reading comprehension is kind of required to this guide lol.

> [!WARNING]
> If you did not read all of that, this is a **Personal Guide**. written by myself, for me. Basically explaining how I made Minecraft run faster. And information here may be incorrect, which if you find any, feel free to [create an issue](https://github.com/weskified/weskers-minecraft-optimization/issues) mentioning that!

## Table of Contents
- [System Specifications](#system-specifications)
  - [Computer](#computer)
  - [Dedicated Server](#dedicated-server)
- [Client Optimizations](#client-optimizations)
  - [Java](#java)
    - [Different types of Java Distributions](#different-types-of-java-distributions)
    - [Does the Java Version matter?](#does-the-java-version-matter)
  - [Installing Java](#installing-java)
    - [Installing via Launcher](#installing-via-launcher)
    - [Installing Manually](#installing-manually)
  - [Java Arguments (Java 25+)](#java-arguments-java-25)
    - [Java Arguments Explanation](#java-arguments-explanation)
      - [Sources](#sources)
  - [Minecraft Mods](#minecraft-mods)
- [Other Client Optimizations](#other-client-optimizations)
  - [Huge Pages](#huge-pages)
    - [There are Two Types of Huge Pages](#there-are-two-types-of-huge-pages)
    - [What's the benefit for enabling this on Minecraft?](#whats-the-benefit-for-enabling-this-on-minecraft)
  - [Turning on Huge Pages in your OS](#turning-on-huge-pages-in-your-os)
    - [Windows](#windows)
    - [Linux (temporary)](#linux-temporary)
    - [Linux (permanent, service based)](#linux-permanent-service-based)
  - [Electron Applications](#electron-applications)
    - [The First Reason](#the-first-reason)
    - [The Second Reason: CPU go brrrrrrrrrrrr (in a bad way)](#the-second-reason-cpu-go-brrrrrrrrrrrr-in-a-bad-way)
    - [The Third Reason: Your GPU is Crying](#the-third-reason-your-gpu-is-crying)
    - [The Fourth Reason: Your Memory belongs to the Electron Application now](#the-fourth-reason-your-memory-belongs-to-the-electron-application-now)
- [Server Optimizations](#server-optimizations)

## System Specifications
These are my system specs, and mainly used as a reference for any future benchmarks that may happen soon. (If i ever get into doing them) If your system specs are worser than mine, expect less performance. Same thing when you have better specs than mine.

### Computer
The computer I use everyday, and definetly the computer I use to run minecraft on.
- **CPU**: Intel i3-10100
- **GPU**: AMD RX580 2048SP
  - This graphics card is overclocked to **1450MHz core clock**, with a maximum power draw of **150W**.
- **RAM**: Kingston Beast (16GB) + Kingston Fury (8GB) @ 2666MHz
- **MOBO**: Gigabyte H510M K V2
- **OS**: CachyOS Linux (x64)

### Dedicated Server
Currently I do not own a Dedicated Server to run a minecraft server with, but this is here just in case I ever get one.

## Client Optimizations
This part of the guide mainly focuses on Client Optimizations, so the things that will be talked about here would probbably be the usual stuff that most people know, along with a bit of unusualness. If you are just a casual player, a modpack maker, or whatever. This part of the guide is what you're looking for!

### Java
The very first obvious thing to configure when having a fresh install of Minecraft is to configure Java. And one of them is configuring the Arguments that would be used to run the JRE (Java Runtime Environment). And that itself is a problem when configured incorrectly, so this section will cover everything about HOPEFULLY configuring it properly, along with some new funky stuff that I have done in order to get the best performance as I can.

#### Different types of Java Distributions
First of all, we need the *perfect* Java Distribution that we will be using to play Minecraft on. And with the Distributions itself, there are a lot of Distributions to choose from. While some of them are good, there are also bad ones where it completely converts minecraft into a screenshare. *coughs on Azul Prime*

To save you the time to look up which ones are the good ones, The two Java Distributions that you can choose from is either [Azul Zulu](https://www.azul.com/downloads) for great performance, and [Adoptium Termuin](https://adoptium.net/temurin/releases) for stability. While they belong to different vendors, and are completely different distributions, they don't really have any differences in performance. So for my recommendation, use [Azul Zulu](https://www.azul.com/downloads) instead!

#### Does the Java Version matter?
Nah, Here's two screenshots of me using [Azul Zulu for Java 26 (26-ea+22)](https://www.azul.com/downloads/?version=java-26-ea&package=jdk#zulu) in Minecraft 1.13 (vanilla). Which works completely fine without any issues!
<img width="1221" height="634" alt="image" src="https://github.com/user-attachments/assets/a882cd2b-6394-4f24-a637-8b84d5f774e8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eb36eba5-2e7e-4e75-b1d5-d231c9f54d09" />

Am i right? ..well, im not really sure about this. But all i know that it works, and that **Java is backwards compatible**. So that you can completely ignore the recommended version that Mojang has given the specific version of minecraft with.

### Installing Java
For this guide, we will be installing the latest LTS (Long-Term Support) version of Java, which would be Java 25! With a tutorial on how to do it via Launcher, or manually installing it and setting it via Launcher.

#### Installing via Launcher
I think pretty much all of us uses a launcher to run Minecraft. And the most popular one that people know is [Prism Launcher](https://prismlauncher.org/download/) and [Modrinth](https://modrinth.com/app). But for this guide, we will be using Prism Launcher instead, as it is the best universal launcher for Minecraft to ever exist.

Anyways, The installation should be pretty simple, go to **Settings**.
<img width="1244" height="1128" alt="image" src="https://github.com/user-attachments/assets/5f9a02c4-cb1a-4639-a8bd-1865567a1698" />

Select **Java**, go to Installations and Download either Adoptium or Azul Zulu, select Java 25 (the latest one). Then press **Download**.
<img width="2256" height="1624" alt="image" src="https://github.com/user-attachments/assets/48d8eece-63de-4ca4-b264-2b81c85ef240" />

aaaaaaaaaaaaaaaand you're pretty much set from here. Just make sure that the version that you're using is the correct one that you downloaded, and make sure to check out the two checks for the Java Version that you're running.

> [!NOTE]
> For Modrinth Launcher. The launcher automatically downloads from Azul Zulu. you cannot change this. (It doesn't matter anyways)

#### Installing Manually
So you want to give yourself a hard time just to install Java for Minecraft? cool! This section exists for people like you.

You can install [Adoptium](https://adoptium.net/temurin/releases?version=25&os=any&arch=any) or [Azul Zulu](https://www.azul.com/downloads/?version=java-25-lts&package=jdk#zulu), then download the JRE compatible with your system. (These links to the Java version so you don't have to, just make sure to select the actual operating system that you're using. you should NOT get this process wrong)

Once you downloaded Java in your computer, it should be at the **Downloads** folder. Select the ZIP file that downloaded which has the packaged JRE inside, and extract it to somewhere neat. (Like `C:/java` or something. up to you) And then copy the `javaw.exe` (windows) or `java` (linux) path inside the `bin` folder to your launcher.

It should look something like this:
<img width="1080" height="610" alt="image" src="https://github.com/user-attachments/assets/b11bfad2-3563-4c16-adeb-1a9b70049224" />
(This was done by automatic download from the launcher, but you know what to do anyways.. right?)

### Java Arguments (Java 25+)
These Java arguments are the ones that I use to play Minecraft with. It's not really the best, but it's enough to make Minecraft run a lot faster and better in my system!

> [!CAUTION]
> My arguments for Java uses **Huge Pages**. I recommend that you turn this thing on in your system!\
> The argument does nothing when you leave it on the list. Which just leaves a warning on the logs everytime you boot Minecraft.
>
> To know more about **Huge Pages**, and how to enable it, Visit [this section](#huge-pages).

> [!CAUTION]
> For windows, replace `-XX:+UseTransparentHugePages` with `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`!\
> In linux, I do not recommend using the two together at all.

```
-XX:+UseZGC -XX:+UseCompactObjectHeaders -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:+PerfDisableSharedMem -XX:+UseTransparentHugePages -XX:-DontCompileHugeMethods -XX:+UseStringDeduplication -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1
```

#### Java Arguments Explanation
> [!WARNING]
> Any arguments marked with ⚠️ can be a problem over a specific issue. So if you run into any, feel free to remove them! (if actually related)

- `-XX:+UseZGC`: Enables the Z Garbage Collector, a low-latency GC that does most of its work concurrently with your application threads. This means Minecraft won't freeze as much during garbage collection because ZGC doesn't need to pause everything to clean up memory.

- `-XX:+UseCompactObjectHeaders`: A newer feature in Java 25 that reduces the memory overhead of object headers in the heap, resulting in lower memory usage and better performance overall.
  
- ⚠️ `-XX:+DisableExplicitGC`: Tells the JVM to ignore manual garbage collection calls (like `System.gc()`) that might be in Minecraft mods or code. This prevents poorly-timed GC from messing with ZGC's optimized collection schedule. The downside is that some code relying on explicit GC might not reclaim memory as expected, but this is rarely an issue unless you're running on extremely limited memory.
  
- `-XX:+AlwaysPreTouch`: Forces the JVM to actually touch and initialize all allocated heap memory during startup, rather than doing it lazily as needed. This makes startup slower but can improve runtime performance by avoiding page faults later.
  
- `-XX:+PerfDisableSharedMem`: Disables the JVM from writing performance statistics to `/tmp/hsperfdata_*` shared memory files. This reduces disk I/O operations that can cause microstutters.
  
- `-XX:+UseTransparentHugePages` or `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`: Enables the use of larger memory pages (2MB instead of the default 4KB). This reduces the overhead of managing the page table since the JVM needs to track fewer, larger pages instead of tons of tiny ones.
  
- `-XX:-DontCompileHugeMethods`: Allows the JIT compiler to compile very large methods that it would normally skip. By default, the JVM avoids compiling huge methods because they take a long time to compile, but this flag forces it to compile them anyway for potential performance gains.

- `-XX:+UseStringDeduplication`: No explanation available for this yet

- `-XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M`: These control the code cache where the JVM stores compiled native code. Java starts as bytecode, then the JIT compiler converts frequently-used methods into faster native machine code. These flags allocate 400MB total for the code cache (vs the default ~240MB) and divide it into segments for different types of compiled code.
  
- `-XX:NmethodSweepActivity=1`: Controls how aggressively the JVM cleans up old/unused compiled code from the code cache. A value of 1 is very aggressive, meaning it'll more readily remove old compiled methods to make room for new ones.

##### Sources
- https://exa.y2k.diy/garden/jvm-args/ (because of [#1](https://github.com/weskified/weskers-minecraft-optimization/issues/1))
- People from [BruceTheMoose's discord server](https://discord.gg/u3NgjC23Qm) for some advice over these :3

### Minecraft Mods
> [!NOTE]
> I am currently making my own Optimization modpack for Mineraft!\
> This optimization modpack's primary focus is to make different parts of minecraft run faster, perform better, instead of mainly focusing on Rendering and Singleplayer performance. And pretty much differentiates from other optimization modpacks out there due to the main reason of making this at the first place is to be used as a base for my Minecraft Modpacks. So I don't have to keep on configuring the same mod again and again for each new modpacks I make which is kind of tiring and repetetive.
> 
> [Download the Modpack](https://modrinth.com/modpack/woof!)


## Other Client Optimizations
This section mainly focuses more on your system in order to make Minecraft run much more faster, along with yapping about other stuff.

### Huge Pages
Huge Pages allows memory intensive applications like Minecraft to run a lot faster! This works because of how your computer manages memory. And to make you understand the concept of huge pages even better (hopefully), the following yap below is an explanation that I think would be perfect in this case.

---

In Minecraft, the entire world is divided into chunks. Each chunk stores a 16x16 block section that goes from bedrock to build height. These are not loaded all at once. The game loads and unloads chunks as you move around.

And funnily enough, Your computer does the exact same thing with memory! Instead of calling them "chunks", computers call them "pages". Each page stores about 4KB of data. Just like Minecraft keeps track of which chunks are loaded, your computer keeps track of every single memory page.

If you've noticed when you move around in your world, chunks can load very slowly. And the reason for that is you're loading new "pages" of memory and unloading old "pages" which all of them are very small 4KB pages. Think of the pages like 16x16 chunks of a world. The processor is struggling to find each individual page of memory to unload and load. because it has to go through small pages of them, which is very hard to keep track of.

When chunks load slowly in Minecraft, the computer has to find which old pages (old chunks) to remove from RAM, fetch new pages (new chunks) from storage, and keep everything organized so the game knows where each chunk is located.

But with enabling **Huge Pages**, it basically converts an entire Minecraft chunk of 16x16 into 64x64, or potentially even more! In our case, allocating about 2MB for each page of memory, which makes:
- Your processor no longer keeps track of tiny 4KB pages, and tracks huge 2MB pages instead.
- Your processor has a special cache named [TLB](https://en.wikipedia.org/wiki/Translation_lookaside_buffer) that remembers where these pages are. And by enabling **Huge Pages**, each pages cover way more memory, so it finds it a lot faster and way more often.
  - Think of a speedrunner finding the stronghold with just Ender Eyes, while this is the usual way that our system functions with just 4KB of pages, finding the stronghold is usually slower this way. and this process can be made faster if we enable Huge Pages, and in a Minecraft way, the speedrunner uses Ninjabrain in order to find the stronghold faster.
- Applications that uses a lot of RAM spends less time managing memory and more time to actually do other things instead.

#### There are Two Types of Huge Pages
On **Windows**, weellll... they're called **Huge Pages**.\
On **Linux**, they're called the same in Winslop, but the much better version of it is called **Transparent Huge Pages** or **THP**

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
Electron Applications are bundled with Chromium. We've been using them a lot, and to be completely honest they aren't really nice.. especially to our very poor computers. And if you're wondering why.. here's a few reasons why!

#### The First Reason
Every electron application that you run is literally running a browser ***separately***. Consider Discord, Spotify, etc. as a browser now that you know that! And your poor processor is like that one guy in a traffic jam trying to fix it.

#### The Second Reason: CPU go brrrrrrrrrrrr (in a bad way)
Every electron application runs on JavaScript (node.js for things to work). If that wasn't obvious enough for a website... while they're pretty fast, it's still interpreted code doing way more work than native code. It's like you (the Processor) is translating a speech from English to German and then Chinese (or Mandarin) just to make the other side of the application to understand what you're actually trying to do. On top of that, each app spawns 6-8 separate processes that constantly talk to each other, freezes the whole UI when JavaScript gets busy (like loading a huge playlist on spotify), and brings its own duplicate copy of every library so five apps means your CPU does the same job five times over for no reason.

##### Solution
Use an alternative version of the application that would load a WebView of the app instead of being loaded as a separate browser or something, like for Discord, use [Vencord](https://vencord.dev/download/) or [Equicord](https://equicord.org/download/)!

Alternatives of Discord like these act a lot differently. Instead of running an Electron application bundled with Chromium, they run a Webview of a page (in this case, Discord: `https://discord.com/app`) which completely removes the problem of using too much CPU, and itself being an electron application in the first place.

This does come with a sacrifice, with you losing Discord RPC unless you have aRPC to come with it. (in this case [GoofCord](https://github.com/Milkshiift/GoofCord) is a nicer alternative)

#### The Third Reason: Your GPU is Crying
Your GPU is crying because of Electron applications. And the reasoning behind this is because each app demands its own GPU rendering pipeline making your graphics card juggle everything inefficiently, spawns dozens of layers for every little animation and UI effect, and stuffs your VRAM with duplicate copies of the same fonts and textures over and over. Five apps might waste **500MB** to like **1GB** of VRAM just rendering the same redundant UI elements separately when they could share resources like normal programs.

##### Solution
If you have an iGPU (Integrated Graphics), you can ask the electron application to use your iGPU instead of your GPU, which definetly frees up some space for the GPU to render actual important stuff with!

Another way to do this is to instead disable **Hardware Acceleration** to the Electron Application.

<img width="907" height="340" alt="image" src="https://github.com/user-attachments/assets/faca4045-8633-453b-bfd5-84265cdd521f" />

> [!WARNING]
> Disabling **Hardware Acceleration** completely forces the electron application to render elements using your CPU instead. This is recommended if you have a pretty powerful CPU but it doesn't come with an iGPU.

#### The Fourth Reason: Your Memory belongs to the Electron Application now
Each Electron app demands **200MB to 400MB of RAM** just to exist because it's packaging an entire browser and JavaScript runtime (node.js), then progressively leaks memory over time (Discord increases to over a gigabyte after running for days for some fucking reason), hoards thousands of chat messages and UI elements you'll never look at again (lol), and forces your RAM to hold four complete duplicate copies of Chromium, image decoders, fonts, and audio libraries when you run multiple apps. so your system starts using swap memory making everything slow, generates heat that throttles your CPU and GPU, drains your laptop battery with constant background activity, and pollutes your CPU cache with redundant code instead of your actual working data... awesome!!

## Server Optimizations
This is currently unavailable for now, as I don't own a server lol. But im kind of guessing that maybe using G1GC and GraalVM would be cool to use especially on a modded environment?? I dont think servers should even use ZGC even.
