# Wesker's Minecraft Optimization Guide
This serves as a **personal guide** to how I optimized Java to run faster in minecraft!

- Install [GraalVM](https://www.graalvm.org/downloads)!
  - Preferrably GraalVM 25 for minecraft versions 1.21 and up!
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
    - <img width="407" height="227" alt="image" src="https://github.com/user-attachments/assets/85e0971c-715a-48fd-870c-23dd8d22b0a3" />
  - Modrinth: 
    - <img width="656" height="214" alt="image" src="https://github.com/user-attachments/assets/12260808-4053-453d-87a2-d1465ed5ad9b" />
- Put the following JVM Arguments into your launcher. (it's kept updated until no further optimizations can be made!)
```
-XX:+UseG1GC -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+ParallelRefProcEnabled -XX:+PerfDisableSharedMem -XX:+UseCompactObjectHeaders -XX:+UnlockExperimentalVMOptions -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1HeapWastePercent=5 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:MaxGCPauseMillis=200 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:+UseStringDeduplication -XX:ReservedCodeCacheSize=512M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=250M -XX:NonProfiledCodeHeapSize=250M -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -XX:+OmitStackTraceInFastThrow -XX:AllocatePrefetchStyle=3 -XX:+UseFastUnorderedTimeStamps -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:+UseJVMCICompiler -XX:+EagerJVMCI -Djdk.graal.CompilerConfiguration=enterprise -Djdk.graal.TuneInlinerExploration=1 -Duser.language=en -Dfile.encoding=UTF-8
```
