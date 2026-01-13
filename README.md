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

## Sources
These are the sources of where I made my flags!
- https://exa.y2k.diy/garden/jvm-args/ (because of [#1](https://github.com/weskified/weskers-minecraft-optimization/issues/1))
