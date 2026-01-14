# Wesker's Minecraft Optimization Guide
This serves as a guide on how I made the Minecraft Client run faster and better! With the use of Java Arguments to get the intended performance gain I can squeeze out to minecraft!

> [!WARNING]
> These java arguments are not built for **Minecraft Servers**! They are mainly built for the Minecraft Client and is not tested to a server environment.

## Setup
In order to get actual performance gain, We will be using **Java 25** of any distribution of your choice! But the most popular ones today are mainly [Adoptium Eclipse Termuin](https://adoptium.net/temurin) and [Azul Zulu](https://adoptium.net/temurin)!
> My older recommendation being GraalVM 25 is pretty much donezo, so any of these two will work just fine!

The following instructions below is a guide to help you setup minecraft to run faster, so read them properly!

- Install the Java Distribution of your choice.
  - You can do this automatically inside your Launcher, specifically [Prism Launcher](https://prismlauncher.org/). [Modrinth](https://modrinth.com/app) automatically downloads from [Azul Zulu](https://www.azul.com/downloads/?version=java-25-lts&package=jdk#zulu). so once you've done this.. skip to the third step.
  - If you want to do this manually, you can continue reading this.
- Extract the downloaded JDK somewhere, then copy, and paste the executable to your launcher.
  - To do this, extract the downloaded package for the JDK to a specific path.
    - For example, extract `azul_zulu_jre25.0.1` into `C:\java\azul_zulu_jre25.0.1` or something.
    - After that, copy the path for `javaw.exe` (windows) or `java` (linux), can be found inside the `bin` folder.
    - Paste the path to your launcher, and verify that the executable works properly.
      - Prism Launcher:
        - <img width="851" height="408" alt="image" src="https://github.com/user-attachments/assets/346d3de1-8869-44de-ad9a-51a18ac89efb" />
        - Make sure to turn on `Skip Java compatibility checks`!
      - Modrinth: 
        - <img width="984" height="321" alt="image" src="https://github.com/user-attachments/assets/12260808-4053-453d-87a2-d1465ed5ad9b" />
- Enable **Huge Pages** (windows) or **Transparent Huge Pages** (linux) in your OS.
  - Windows
    - To do this, press `Win + R`, type `secpol.msc`, and press `Enter`.
    - Go to `Security Settings` > `Local Policies` > `User Rights Assignment`.
    - Inside that, find the **Lock pages in memory** policy and then double click that policy.
    - Click **Add User or Group**, type your username (the name of the windows user account that you're using currently), then click **OK**.
    - Click **Apply** then restart your computer.
  - Linux
    - Do i even have to?
    - In some distributions, THP is enabled by default. Check if your distribution has it on by default with `cat /sys/kernel/mm/transparent_hugepage/enabled`.
      - If it says `[always] madvise never`, it means it's enabled already.
- Paste the following JVM arguments to your launcher.
  - The JVM arguments are kept updated until no optimizations can be possibly made anymore.

> [!NOTE]
> For the last two arguments for the following args below, `-Duser.language=en -Dfile.encoding=UTF-8` can be removed, but is there by personal preference due to me running into issues with very weird witchcraft with different languages and wonky encoding (rarely) :P

## JVM Arguments for Java 25
```
-XX:+UseZGC -XX:+UnlockExperimentalVMOptions -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+PerfDisableSharedMem -XX:+UseCompactObjectHeaders -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1 -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:+UseTransparentHugePages -XX:AllocatePrefetchStyle=3 -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -Duser.language=en -Dfile.encoding=UTF-8
```

> [!WARNING]
> For windows, replace `-XX:+UseTransparentHugePages` with `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`!
> In linux, I do not recommend merging the two together.

### Args Explanation
> [!WARNING]
> This was written on 1 am, some explanations may not be as accurate from what the arguments actually does, so read carefully and maybe trust your guts if you feel like something is explained wrong here.

- `-XX:+UseZGC`: Enables the ZGC, basically like a garbage collector that doesnt force minecraft to freeze for too long because it has to do it's thing before you can start getting frames again lol
- `-XX:+UnlockExperimentalVMOptions`: Unlocks very high tech and experimental witchcraft.
- `-XX:+AlwaysPreTouch`: Tells the JVM to start muching on the allocated memory that you told it to reserve on startup, with the cost of a slower startup. (since it has to start munching on it first)
- ⚠️ `-XX:+DisableExplicitGC`: Ignores any GC calls (or `gc()`) in minecraft mods or code that may interfere with the garbage collection. But with the cost of vanilla not being able to recover after losing allocated memory. (this is alright to have in the flags, unless you're running minecraft with 128mb which you should definetly remove)
- `-XX:+PerfDisableSharedMem`: Disables the JVM from writing performance data to shared memory files. Like telemetry, but for ram. (it also apparently does a lot of r/w witchcraft, so this prevents that from happening)
- `-XX:+UseCompactObjectHeaders`: Newer feature in Java 25 which "enables a new way for Java objects to be represented in heap that results in less memory usage and better performance."
- `-XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M`: Basically java code starts as bytecode, then the JVM compiles frequently used methods into faster native machine code so minecraft can run faster. with `400MB` allocated of ReservedCodeCacheSize which is a lot more than the normal. (like 240MB i think)
- `-XX:NmethodSweepActivity=1` Controls how the JVM agressively cleans up old compoled code from the code cache, the value set here is very agressive, lower ones makes it less agressive of course.
- `-XX:MaxNodeLimit=240000`: Sets a limit of nodes that the JIT compiler can create during compilation. more nodes = more complex optimizations, with sacrificing more memory usage during compilation.
- `-XX:NodeLimitFudgeFactor=8000`: life support for the argument above, basically a buffer added until it ultimately does an epic fail and gives up.
- `-XX:+UseTransparentHugePages` or `-XX:+UseLargePages -XX:LargePageSizeInBytes=2M`: Enables the use of huge memory pages, reducing the overhead of managing tiny little memory pages since by default all operating systems allocate `4KB`, but this option allows the JVM to allocate `2MB` instead. (of course making managing memory faster or something ???)
- `-XX:AllocatePrefetchStyle=3`: Controls how the JVM prefetches memory when allocating objects. 3 is very agressive, attempting to predict and load memory before its actually needed to reduce latency.
- `-XX:+UseFastUnorderedTimeStamps`: Uses a faster but less prescise method for getting timestamps, because we are not running facebook datacenters that needs every logs to be around 1 picosecond accurate or something lmao
- ⚠️ `-XX:+UseCriticalJavaThreadPriority`: Makes java threads have higher thread priority, can help a lot with latency but may interfere with other programs in your computer, mostly okay unless you're running a very low-end computer.

## Sources
These are the sources of where I made my flags!
- https://exa.y2k.diy/garden/jvm-args/ (because of [#1](https://github.com/weskified/weskers-minecraft-optimization/issues/1))
