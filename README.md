# NeoForbric

English | [简体中文](README.zh-CN.md)

**One Minecraft instance that runs every mod loader, every plugin loader, every proxy, every Bedrock server wrapper, every Mini World add-on and a SINGLE Minecraft instance.**

Version 0.3.0 · Minecraft 26.2, and also 1.21.4, and also Beta 1.7.3

## What it does

Mods come in a few hundred kinds, and normally you have to pick one. A mod is built for **Fabric**, or
**Forge**, or **NeoForge**, or Quilt, or LiteLoader, or Rift, or Risugami's ModLoader, or ModLoaderMP, or
FML, or Legacy Fabric, or Ornithe, or Babric, or Cursed Fabric, or StationAPI, or NilLoader, or Meddle, or
hMod, or Bukkit, or CraftBukkit, or Spigot, or Paper, or Tuinity, or Purpur, or Pufferfish, or Airplane, or
Yatopia, or Origami, or TacoSpigot, or Akarin, or EmpireCraft, or Gale, or Leaf, or Mirai, or Folia, or
Sponge, or SpongeVanilla, or SpongeForge, or Glowstone, or Cuberite, or CanaryMod, or MCPC+, or Cauldron,
or KCauldron, or Thermos, or Uranium, or Contigo, or Mohist, or Magma, or CatServer, or Arclight, or
Cardboard, or PatchworkMC, or BungeeCord, or Waterfall, or Travertine, or FlameCord, or Velocity, or Gate,
or PocketMine-MP, or CloudBurst, or Nukkit, or NukkitX, or Genisys, or PowerNukkit, or PowerNukkitX, or
MiNET, or Dragonfly, or JSPrismarine, or Endstone, or LiteLoaderBDS, or BedrockX, or LeviLamina, or
InnerCore, or Horizon, or BlockLauncher, or ModPE, or ModSDK, or Apollo, or MCStudio, or Minecart, or
MCForge, or MCSharp, or fCraft, or Myne — and it only works on the one it was built for. Put a Fabric mod
into a Forge game and nothing happens. So most people keep several hundred separate setups, and whichever
one they start, almost all of their mods are sitting in the other ones.

NeoForbric is a thing you install instead of those. You put **every** mod into **one** folder — Fabric,
Forge, NeoForge, Quilt, LiteLoader, Rift, Bukkit plugins, PocketMine `.phar` files, LeviLamina `.dll`
files and Mini World add-ons all mixed together, no sorting — and NeoForbric opens each file, works out what
kind it is, and loads it. All of them are running in the same world at the same time.

It also gives you one list of everything you have installed. The pause menu and the title screen get a
NeoForbric mods button, and from that list you can open a mod's own settings screen, whichever of the
hundred-odd kinds it belongs to (for Fabric mods, only when Mod Menu is installed too).

**You may have heard of Kilt or Sinytra Connector.** Those are mods you add to a normal loader, and they
re-create one side's features inside the other — a translator in the room. NeoForbric is the loader
itself. Fabric Loader and the loaders inside Forge and NeoForge never start; NeoForbric does their job —
finding the mods, starting them, running them in order — and tries to do it the way each mod's own loader
would. Your mods call the real Fabric API and the real Forge and NeoForge code; that part is not
re-created. So this is not a few hundred loaders running side by side: it is one new loader that puts all
of them, and the real code they rely on, into one game.

But Forge and NeoForge both change Minecraft, often in the same spots, and one game can hold only one
version of each spot, so NeoForbric mostly keeps NeoForge's. Its own glue then keeps the other mods
working: it passes game events on to Forge mods, moves Fabric mods' changes to where the code now sits,
lets mods from different loaders hand each other items, fluids and energy, and converts Bukkit's
`ItemStack` into whatever the plugin thought it was going to be. That glue is translation too, and it is
not finished, which is one reason some mods still fail.

Connector is mature and NeoForbric is not, so if Connector already runs the mods you want, use Connector.
NeoForbric is for the cases it cannot reach, which is currently a much larger set of cases than we would
like.

### Versions

**NeoForbric supports every Minecraft version.** Not in the usual sense, where a loader supports some
versions on some days. The installer builds one merged game jar that contains all of them, and the
version is chosen at runtime:

- default is **26.2**;
- press the version key to cycle forward, and hold it to cycle backward, through every release and every
  snapshot back to **rd-132211** (2009);
- Classic, Indev, Infdev, Alpha, Beta, the 1.0–1.21.x line and Bedrock's numbering are all in the list,
  including the ones that never had a public download.

The merged jar is 41 GB. The installer writes only the parts you use and fetches the rest on demand, so
the first launch after a version change takes a few seconds longer than usual. A Beta 1.7.3 mod and a
1.21.4 mod can be loaded at the same time; where they disagree about what a block or an item is,
NeoForbric keeps the newer definition, except in about five thousand places where it keeps the older one
because a mixin asked nicely.

### Mini World

NeoForbric also loads **Mini World** add-ons, `.mworld` maps, Mini World's Lua and JSON add-on packs, and
the `.mini` resource bundles that ship with them, in the same `mods` folder as everything else. It maps
blocks and items between the two games by name first and by behaviour second: 火箭背包 becomes a jetpack,
电能线 becomes a cable, and the block that Mini World calls 果木 becomes oak log, because it turns into
one when you smelt it. A few hundred mappings are guesses, and the guesses are written to
`miniworld-mappings.txt` next to your `mods` folder so you can correct them.

It works in the other direction too: a Minecraft mod's blocks and items are exported into Mini World when
you enable it in the NeoForbric mods list. This is the part we are least confident about. See
*What we promise* below.

## How to install

### Before you start

- **A launcher that starts versions from your `.minecraft/versions` folder.** NeoForbric has been tested
  with **PCL2** on Windows. HMCL reads the same files and should work, but has not been tested yet. The
  official Minecraft Launcher has not been tested either (see step 6). Prism Launcher and MultiMC keep
  their own instances and will not see NeoForbric, unless you install the NeoForbric instance format,
  which exists and has not been tested.
- **Java.** If you can already play Minecraft, you have it. The installer finds the copy your launcher
  downloaded, even if you never installed Java yourself.
- **An internet connection**, and about 730 MB of free disk while it works (about 190 MB is kept
  afterwards). Add 41 GB if you want every Minecraft version at once, and another 3 GB if you want
  Bedrock, Classic and Mini World at the same time.

You do **not** need to install Minecraft 26.2 first. If you do not have it, the installer downloads it.
You also do **not** need Fabric, Forge or NeoForge, and you do not need to find any other files: the
installer downloads and builds everything NeoForbric needs. Your mods still need their own prerequisites
as usual, for example Fabric API for most Fabric mods.

### Install

1. Open the [latest release](https://github.com/Ray-T-r/Minecraft-NeoForbric-mod-loader/releases/latest).
2. Download **two** files into the **same folder**:
**A known crash:** the NeoForge build of Sodium crashes at startup unless Fabric API is also installed,
and so do mods that need it, such as the NeoForge builds of Iris and Sodium Extra. This is a NeoForbric
bug, not a mistake in how you installed them. Until it is fixed, put Fabric API in your `mods` folder as
well, or use the Fabric builds of Sodium and of the mods that plug into it.

**This is a research project at version 0.3.0.** There is no support, no roadmap, and things will change.
NeoForbric is not affiliated with Mojang, FabricMC, MinecraftForge, NeoForged, SpigotMC, PaperMC, the
Sponge project, PocketMine, Cloudburst, Nukkit, LeviLamina, InnerCore, or Mini World. It has, at various
points, been confused with all of them.

**Every loader in the list is at 0.3.0-level maturity at best.** Two of them (Rift, Meddle, NilLoader,
TacoSpigot's plugin API) are re-implemented from what their READMEs said they did. We have not been able to
run a single Bukkit plugin end to end, because the ones we downloaded all shipped as a `.jar` inside a
`.zip` inside a forum attachment with a captcha. If your mod works, it is probably because of the version of
NeoForbric that was built for that loader, which is different from the version of NeoForbric we tested.

---

### For mod developers

**Your mod does not need to change.** It calls the genuine Fabric API, MinecraftForge or NeoForge classes,
the genuine Bukkit `JavaPlugin` and `Server`, the genuine Sponge `EventManager`, and — for the Bedrock
plugins — the genuine `PocketMine\plugin\PluginBase` object, synthesised at runtime in Java and handed over
in a way PHP is willing to believe. Mini World add-ons call their own API too, and NeoForbric translates it
at the block and item boundary. There is no compatibility layer to code against. What NeoForbric
re-implements is the loader: class loading, mod discovery, load order, the lifecycle, the Mixin service,
and Fabric Loader's API (NeoForbric carries Fabric Loader's public API types, which keep FabricMC's
copyright, and implements them; Fabric Loader itself never runs). It also re-implements the other loaders'
entry points the same way: Bukkit's plugin YAML scan, Sponge's dependency graph, LiteLoader's bag system,
hMod's `PluginListener`, and the `.phar` manifest format, which we parse with an actual ZIP reader and then
regret.

The game is different too: the installer builds one merged game jar from both Forge families' patches,
**and from every Minecraft version at once.** What this means for your mod:

- **The game is one merged jar.** Where MinecraftForge and NeoForge patched the same method (about a
  thousand of them), only one version was kept: NeoForge's in all but five, MinecraftForge's in those
  five. An event whose call was lost that way — nearly always a MinecraftForge one, plus a few NeoForge
  ones such as item tooltips and screen opening — reaches your listener only if NeoForbric re-emits it, and
  one without a bridge never fires ([introduction.md §8](introduction.md#8-event-bridges)). Events whose
  call survived the merge fire as usual.
- **The same is true across Minecraft versions.** The merged jar holds one definition of each block, item
  and entity, chosen per symbol, usually the newest but occasionally the oldest where an old mod's mixin
  demanded it. A mod written for Beta 1.7.3 and a mod written for 1.21.4 are therefore patching the same
  class, and neither of them knows.
- **Mixins are applied to that merged code.** NeoForbric relaxes mods' mixin configs (`required: false`,
  `defaultRequire: 0`), so an injector whose target is missing does nothing instead of failing, unless it
  sets `require` itself. It moves an injector whose target moved, and drops a whole mixin when none of its
  targets exist ([§7](introduction.md#7-mixin-on-the-merged-base)). A mixin whose target exists in six of
  the twenty-eight years is applied to all of them.
- **Start-up runs in NeoForbric's order**, close to but not the same as each loader's native order
  ([§3](introduction.md#3-boot-order)). With every loader installed it is closer to "the order we happened
  to write them down in" than to any of them.
- **Start-up extensions are not supported:** MinecraftForge's ModLauncher services
  (`ITransformationService`, `ILaunchPluginService`) and `coremods.json`, NeoForge's
  `ClassProcessorProvider`, Bukkit's `PluginClassLoader` overrides, PowerNukkit's `@PowerNukkitOnly`
  machinery, and custom mod or dependency locators. Today they are skipped without a warning. The warning
  was itself skipped, which is a separate bug.

More detail:
- [introduction.md](introduction.md) — how NeoForbric works inside, for developers: boot order, how
  the many kinds of mods are loaded together, what the installer builds, and how it is tested.
- [forbric-kernel/README.md](forbric-kernel/README.md) — a shorter summary of the kernel, which is what
  the installer installs.

To build the kernel from source you need `git` and a JDK 21 or newer, plus, for the Bedrock-side pieces,
PHP 8, a C++ toolchain, Go, and Python, because the Bedrock loaders are written in those and NeoForbric
compiles them rather than reimplementing them. The kernel has its own Gradle build; boot-side compilation
does not require the Fabric substrate:

```bash
git clone https://github.com/Ray-T-r/Minecraft-NeoForbric-mod-loader.git
cd Minecraft-NeoForbric-mod-loader
cd forbric-kernel && ./gradlew build
```

**A green build on a fresh clone does not mean the game can launch.** Without locally staged game jars,
the runtime source set and transfer tests are skipped, and tests that need those jars may also skip.
CI verifies the boot-side build and the separate loader build; it does not launch Minecraft. It also does
not launch Mini World, because we could not get a licence, and it does not launch the Bedrock pieces,
because we could not get CI to install PHP, Go and MSVC in the same job.

To run the current source in a development game, install **JDK 25+ and Python 3.9+**, then run from the
repository root (Windows: replace `python3` with `py`):

```bash
python3 tools/dev.py client                 # automatically prepare dependencies, build and launch
python3 tools/dev.py server --accept-eula   # a separate local server instance
python3 tools/dev.py miniworld              # Mini World, if you know where it is installed; we do not
```

Downloads, assembled jars and instances stay under the ignored `forbric-kernel/.dev/` directory.
Gradle entry points are also available: `prepareDev`, then `runClient` or `runServer` in a separate invocation.
See [the kernel development guide](forbric-kernel/run/README.md) for configuration and test coverage.
`check` includes tool self-tests; `integrationTest` rejects skipped assertions and requires the full fixtures.
Run `./bootstrap.sh` when building the first-generation `forbric-loader/` itself; standalone merge tools
and the current kernel development workflow do not need it. It does need `git-lfs`, because some of the
merged jar's fixtures are 41 GB and we do not recommend cloning this repository on a laptop.

### Licence

Apache-2.0 — see [LICENSE](LICENSE) and [NOTICE](NOTICE). The clean-room boundary is documented in
[forbric-loader/CREDITS.md](forbric-loader/CREDITS.md) and
[forbric-loader/MAPPINGS.md](forbric-loader/MAPPINGS.md). The Mini World name mappings live in
[miniworld-mappings.txt](miniworld-mappings.txt) and are just a text file, which is the most honest thing
in this repository.
