# Wesker's Minecraft Optimization Guide
This repository serves as a personal guide on how to make Minecraft run a lot faster and better! This guide will talk about 3 different categories for optimizations. Some may be explained very bad or some not, but at least written enough to make it understandable for my funky small brain factory that keeps me working for like... idk.

> [!WARNING]
> You have been warned. This guide is **a personal guide**. Anything written here is what I do in order to optimize minecraft, and things may make your minecraft run faster or not.. but usually depends.

## Table of Contents
- [Java Optimizations](#java-optimizations)
  - [Installing Java](#installing-java)
    - [Manually](#manually)
    - [From Launcher](#from-launcher)
  - [Setting up Huge Pages in your Operating System](#setting-up-huge-pages-in-your-operating-system)
    - [Windows (Huge Pages)](#windows-huge-pages)
    - [Linux (temporary)](#linux-temporary)
    - [Linux (permanent)](#linux-permanent)
  - [JVM Arguments](#jvm-arguments)
    - [Args Explanation](#args-explanation)
    - [Sources](#sources)
- [Minecraft Optimization Mods](#minecraft-optimization-mods)
  - [Mods](#mods)
- [Other Optimizations](#other-optimizations)
      

## Java Optimizations
Optimizing Java is pretty much the first thing that *most* people have in mind when playing minecraft. And that has also been a habit of me to do, and has really helped increasing the performance of mineraft overall instead of having to play through a presentation in a block game.

### Installing Java
In order to get some cool gamer FPS gains (or performance), installing juuuuuuust the right Java version is really nice, and for this guide you *will* be installing **Java 25** (at the time of writing), along with a different Java Distribution, with that being [Adoptium Termuin](https://adoptium.net/temurin/releases?version=25&os=any&arch=any) or [Azul Zulu](https://www.azul.com/downloads/?version=java-25-lts&package=jdk#zulu) (i personally use this one).

#### Manually
Once you downloaded Java in your computer, you can extract the downloaded package to a specific location in your computer, Copy the path, then paste the location of `javaw.exe` (windows) or `java` (linux) to your launcher.

It should look something like this:
<img width="1080" height="610" alt="image" src="https://github.com/user-attachments/assets/b11bfad2-3563-4c16-adeb-1a9b70049224" />
(This was done by automatic download from the launcher, but you know what to do anyways.. right?)

#### From Launcher
Your launcher should be able to just automatically download these two for you, specifically, [Prism Launcher](https://prismlauncher.org/). Which gives you the option to choose either one of them. [Modrinth](modrinth.com/app) automatically installs [Azul Zulu](https://www.azul.com/downloads/?version=java-25-lts&package=jdk#zulu) by default and you cannot do anything about it lmao.

### Setting up Huge Pages in your Operating System
This step is pretty much optional, but highly recommended! As this has some noticable changes from the client, from what i've observed, FPS stabilizes a lot faster!

#### Windows (Huge Pages)
> [!IMPORTANT]
> This completely enables Huge Pages **PERMANENTLY** in your system! This will basically just make performance better for Memory-intensive applications like Minecraft.

- Press `Win + R`, type `secpol.msc`, and press `Enter`.
- Go to `Security Settings` > `Local Policies` > `User Rights Assignment`.
- Inside that, find the **Lock pages in memory** policy and then double click that policy.
- Click **Add User or Group**, type your username (the name of the windows user account that you're using currently), then click **OK**.
- Click **Apply** then restart your computer.

#### Linux (temporary)
```
echo advise | sudo tee /sys/kernel/mm/transparent_hugepage/shmem_enabled
```

> [!NOTE]
> To turn this off, just replace `echo advise` to `echo never` once you're done playing minecraft. Or just reboot your entire system if you feel like it. :3

#### Linux (permanent)
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
(or reboot your system)

### JVM Arguments
This is the Java Arguments that I currently personally use, and has actually made the performance of minecraft a lot better!
```
-XX:+UseZGC -XX:+UseCompactObjectHeaders -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:+PerfDisableSharedMem -XX:+UseTransparentHugePages -XX:-DontCompileHugeMethods -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1 -Duser.language=en
```

> [!WARNING]
> For windows, replace `-XX:+UseTransparentHugePages` with `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`!
> In linux, I do not recommend merging the two together.

#### Args Explanation
> [!WARNING]
> This was written on 1 am, some explanations may not be as accurate from what the arguments actually does, so read carefully and maybe trust your guts if you feel like something is explained wrong here.

- `-XX:+UseZGC`: Enables the ZGC, basically like a garbage collector that doesnt force minecraft to freeze for too long because it has to do it's thing before you can start getting frames again lol
- `-XX:+UseCompactObjectHeaders`: Newer feature in Java 25 which "enables a new way for Java objects to be represented in heap that results in less memory usage and better performance."
- ⚠️ `-XX:+DisableExplicitGC`: Ignores any GC calls (or `gc()`) in minecraft mods or code that may interfere with the garbage collection. But with the cost of vanilla not being able to recover after losing allocated memory. (this is alright to have in the flags, unless you're running minecraft with 128mb which you should definetly remove)
- `-XX:+AlwaysPreTouch`: Tells the JVM to start muching on the allocated memory that you told it to reserve on startup, with the cost of a slower startup. (since it has to start munching on it first)
- `-XX:+PerfDisableSharedMem`: Disables the JVM from writing performance data to shared memory files. Like telemetry, but for ram. (it also apparently does a lot of r/w witchcraft, so this prevents that from happening)
- `-XX:+UseTransparentHugePages` or `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`: Enables the use of huge memory pages, reducing the overhead of managing tiny little memory pages since by default all operating systems allocate `4KB`, but this option allows the JVM to allocate `2MB` instead. (of course making managing memory faster or something ???)
- `-XX:-DontCompileHugeMethods`: i dont know what this does... yet. just following an advice, but may remove it soon if it doesn't do anything.
 `-XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M`: Basically java code starts as bytecode, then the JVM compiles frequently used methods into faster native machine code so minecraft can run faster. with `400MB` allocated of ReservedCodeCacheSize which is a lot more than the normal. (like 240MB i think)
 - `-XX:NmethodSweepActivity=1` Controls how the JVM agressively cleans up old compoled code from the code cache, the value set here is very agressive, lower ones makes it less agressive of course.

#### Sources
These are the sources of where I made my flags!
- https://exa.y2k.diy/garden/jvm-args/ (because of [#1](https://github.com/weskified/weskers-minecraft-optimization/issues/1))

## Minecraft Optimization Mods
Minecraft mods are one of the popular and most well known thing that probbably most Minecraft players already know! And this section covers a whole list of minecraft optimization mods that I have been using myself!

> [!NOTE]
> I am currently maintaining a Minecraft Modpack that focuses on completely optimizing different parts of minecraft! Which focuses more on just FPS increase, surely it MAY be the best, right?
>
> Install the [woof! Modpack on Modrinth](https://modrinth.com/modpack/woof!)

### Mods

## Other Optimizations
This category mainly focuses outside minecraft, as it also has to do something with your system.

something something just turn off background processes like your browser, discord, etc.
