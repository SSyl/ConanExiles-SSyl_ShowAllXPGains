# Show All XP Gains

A small quality-of-life mod for **Conan Exiles (Enhanced / UE5)** that makes the floating "+XP" gain popup appear for *all* experience gains, not just large ones.

> [Subscribe on Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3748062292) (id `3748062292`).

## What it does

By default the game hides any XP gain below a threshold of 20, so small gains (a few XP from a pick swing, or any gain on a low-rate server) never show a popup even though the XP is still awarded. This mod lowers that threshold so every gain of at least 1 XP shows.

It is purely a client-side UI change. It does not alter how much XP you earn. It only changes whether the popup is drawn.

## How it works

The mod is a single **base-asset override** of the game's experience-gauge widget.

- Asset: `WBP_ExperienceGauge` (`/Game/UI/Widgets/HUD/StatIndicators/`).
- Variable: `MinimumVisualizationThreshold`, default `20.0`, read in one place (the `HandleXPChanged` function) as `Delta > MinimumVisualizationThreshold`.
- Change: the default is lowered to `0.999`, so any integer gain of 1 or more shows while 0-XP ticks stay hidden. Any value in `[0, 1)` behaves this way. `1.0` would hide gains of exactly 1, and negatives would draw a "+0".

Because it overrides the base widget directly, it conflicts with any other mod that edits the same widget. Whichever loads last wins.

## Compatibility

Originally built against Conan Exiles Enhanced 1.2.1. Verified working on 1.3.

## Multiplayer

Conan enforces strict mod-matching. To use this on a server, it must be in the server's mod list **and** installed on every connecting client. Singleplayer needs no extra setup.

## Repository contents

```
modinfo.json                                              mod manifest (name, version, Workshop id)
preview.png                                               Workshop preview image
Local/CookInfo.ini                                        tells the DevKit which asset to cook
Content/UI/Widgets/HUD/StatIndicators/
    WBP_ExperienceGauge.uasset                            the overridden widget (binary)
```

`WBP_ExperienceGauge.uasset` is a binary Unreal asset. GitHub cannot show a meaningful diff of it, and changes cannot be reviewed line by line. To inspect it, open it in the Conan Exiles DevKit, or use the editor's built-in Blueprint diff tool against history.

## Building from source

Requires the **Conan Exiles Enhanced DevKit** (from Steam).

1. Copy this repository's contents into `<DevKit>/UE4/Content/Mods/ShowAllXPGains/`, so `modinfo.json` sits at the mod folder root.
2. Launch the DevKit (`-ModDevKit`).
3. In the mod-kit window, open **Choose Assets For Cook** and add `WBP_ExperienceGauge`.
4. **Build Mod.** The wrapper `.pak` is written under `<DevKit>/UE4/Saved/Mods/ShowAllXPGains/Output/`.
5. Test locally by dropping that `.pak` into `<Conan Exiles>/ConanSandbox/Mods/` and enabling it in the in-game Mods menu, or upload it to Steam Workshop from the mod-kit.
