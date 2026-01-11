# Wesker's Minecraft Optimization Guide
This serves as a guide to how I have managed to make minecraft run better, using GraalVM and custom java arguments in order to get the intended performance gain I can squeeze out more to minecraft!

> [!WARNING]
> These java arguments are not made for **Minecraft Servers**!!!\
> The java arguments in this repository is made only for the minecraft client. Which has a lot of agressive optimizations that may not be applicable for a server environment.

- Install [GraalVM](https://www.graalvm.org/downloads)!
  - Preferrably GraalVM 25 for minecraft versions 1.21 and up!
  - I am unsure about versions lower than 1.21, as this is the only version I have tried it with!
- Enable **Huge Pages** in your **Operating System**
  - Windows
    - To do this, press `Win + R`, type `secpol.msc`, and press `Enter`.
    - Go to `Security Settings` > `Local Policies` > `User Rights Assignment`.
    - Inside that, find the **Lock pages in memory** policy and then double click that policy.
    - Click **Add User or Group**, type your username (the name of the windows user account that you're using currently), then click **OK**.
    - Click **Apply** then restart your computer.
  - Linux
    - Not sure how, but I'm pretty sure you already know how to. :3
- Extract GraalVM somewhere, and copy the path to the `javaw` executable and paste it in your launcher as default.
  - For example, `D:\java\graalvm-jdk-25.0.1+8.1\bin\javaw.exe`.
  - Prism Launcher:
    - <img width="611" height="341" alt="image" src="https://github.com/user-attachments/assets/85e0971c-715a-48fd-870c-23dd8d22b0a3" />
    - Make sure to turn on `Skip Java compatibility checks`!
  - Modrinth: 
    - <img width="984" height="321" alt="image" src="https://github.com/user-attachments/assets/12260808-4053-453d-87a2-d1465ed5ad9b" />
- Put the following JVM Arguments into your launcher, depending on your Java Version.
  - The following arguments are updated until no further optimizations can be made.

## JVM Arguments for GraalVM 25
```
-XX:+UseG1GC -XX:+UnlockExperimentalVMOptions -XX:MaxGCPauseMillis=120 -XX:G1HeapRegionSize=8M -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1ReservePercent=20 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=10 -XX:G1HeapWastePercent=5 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:G1RSetUpdatingPauseTimePercent=0 -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+PerfDisableSharedMem -XX:+UseCompactObjectHeaders -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1 -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -XX:AllocatePrefetchStyle=3 -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+UseJVMCICompiler -XX:+EagerJVMCI -Djdk.graal.CompilerConfiguration=enterprise -Djdk.graal.TuneInlinerExploration=1 -Duser.language=en -Dfile.encoding=UTF-8
```

## JVM Arguments for GraalVM 21
I currently have an actual list of Java arguments available in my own personal server, although they are currently not here as I have been focusing more on GraalVM 25 for now, modifying it until no further possible optimizations can be made (i am trying so hard rn), and trying to clean up some unused args if any, or args that is mainly useless or does nothing.

## Sources
These are the sources that I have used to make the jvm args!
- https://www.graalvm.org/latest/reference-manual/java/options/
