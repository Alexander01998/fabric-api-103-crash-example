**Update:** Mystery solved. This whole crash is just an extra-sneaky cache bug. Run any Gradle command with `--refresh-dependencies` (for example `./gradlew runServer --refresh-dependencies`) and it goes away. Any other method of clearing your cache, like deleting your .gradle folder or deleting the various cache folders in your repository, does not work for some odd reason.

It's unclear why multiple people got the exact same cache bug right after updating to that exact Fabric API version, or which part of the cache actually got corrupted to cause this weird stacktrace, but at this point I can't be bothered to find out. I'm archiving this repo. Feel free to keep digging if you're curios.

# Fabric API 0.103 Crash Example Mod

This is a stripped-down version of WI Zoom that does nothing except print hello world on start. For some reason this, like many other mods, crashes when used with Fabric API 0.103+1.21.1 but works fine if you downgrade to 0.102.1.

Starting the server crashes immediately. Starting the client crashes when you enter a singleplayer world.

The stacktrace doesn't really tell you anything useful:

```
java.lang.ClassCastException: class com.mojang.datafixers.util.Pair cannot be cast to class net.minecraft.registry.entry.RegistryEntryList$Named (com.mojang.datafixers.util.Pair and net.minecraft.registry.entry.RegistryEntryList$Named are in unnamed module of loader net.fabricmc.loader.impl.launch.knot.KnotClassLoader @6adede5)
	at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.accept(ForEachOps.java:184)
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
	at java.base/java.util.IdentityHashMap$EntrySpliterator.forEachRemaining(IdentityHashMap.java:1628)
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509)
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
	at java.base/java.util.stream.ForEachOps$ForEachOp.evaluateSequential(ForEachOps.java:151)
	at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.evaluateSequential(ForEachOps.java:174)
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.base/java.util.stream.ReferencePipeline.forEach(ReferencePipeline.java:596)
	at net.fabricmc.fabric.impl.tag.convention.ConventionLogWarnings.lambda$setupLegacyTagWarning$1(ConventionLogWarnings.java:237)
	at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.accept(ForEachOps.java:184)
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
	at java.base/java.util.Spliterators$ArraySpliterator.forEachRemaining(Spliterators.java:1024)
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509)
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
	at java.base/java.util.stream.ForEachOps$ForEachOp.evaluateSequential(ForEachOps.java:151)
	at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.evaluateSequential(ForEachOps.java:174)
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.base/java.util.stream.ReferencePipeline.forEach(ReferencePipeline.java:596)
	at net.fabricmc.fabric.impl.tag.convention.ConventionLogWarnings.lambda$setupLegacyTagWarning$2(ConventionLogWarnings.java:235)
	at net.fabricmc.fabric.api.event.lifecycle.v1.ServerLifecycleEvents.lambda$static$2(ServerLifecycleEvents.java:49)
	at net.minecraft.server.MinecraftServer.handler$zeo000$fabric-lifecycle-events-v1$afterSetupServer(MinecraftServer.java:3143)
	at net.minecraft.server.MinecraftServer.runServer(MinecraftServer.java:668)
	at net.minecraft.server.MinecraftServer.method_29739(MinecraftServer.java:281)
	at java.base/java.lang.Thread.run(Thread.java:1583)
```

This mod uses Yarn mappings, but @Erdragh has run into the same issue when using mojmaps.

I've tried changing the following other variables and found that they make no difference:
- Fabric Loader version
- Gradle version
- mixin compatibilityLevel

The same issue does NOT happen with:
- Minecraft 23w35a and Fabric API 0.103.1+1.21.2.
- The 1.21.1 version of fabric-example-mod

At this point I am out of ideas for what might be causing the crash.
