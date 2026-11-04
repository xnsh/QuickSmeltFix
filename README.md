This is a [fork](https://en.wikipedia.org/wiki/Fork_(software_development)) (i.e., a copy with changes) of [WheteThunger's QuickSmelt fork](https://github.com/WheteThunger/QuickSmelt), which is itself a fork of the QuickSmelt plugin on uMod.

https://umod.org/plugins/quick-smelt

Original authors: misticos (uMod) and WhiteThunder (fork). All credit for the plugin goes to them.

## Changes in this fork

This fork adds one compatibility fix on top of WheteThunger's version 5.1.16:

- **Fixed compile error on Rust builds that removed `ItemModCookable.CanBeCookedByAtTemperature`.** Some recent Rust builds removed this method, which caused the plugin to fail to compile with:

  ```
  'ItemModCookable' does not contain a definition for 'CanBeCookedByAtTemperature' [CS1061]
  ```

  The direct call was replaced with a `CanCookAt` helper that:
  - Calls `CanBeCookedByAtTemperature` at runtime if it exists, so behavior is unchanged on Rust builds that still have it
  - Otherwise falls back to the cookable's `lowTemp` / `highTemp` fields
  - Allows cooking if neither is found, instead of throwing an error
  - Caches the lookup after the first call, so there is no per-tick cost

No other behavior or config changes were made.

## Differences from uMod version (from WheteThunger's fork)

- Smelting/cooking animations are actually accurate
- Smelting/cooking multiple items at once does not increase efficiency (like vanilla)
- Multipliers are baselined against vanilla, meaning 1.0 multiplier is actually equivalent to vanilla
- Works with electric furnaces
- Furnaces started via electricity or igniters will function the same as furnaces started directly by a player (uMod version will not detect this, causing furnaces to use vanilla rates in those cases)
- The `OnOvenCook` and `OnOvenCooked` hooks are called correctly, allowing for compatibility with other plugins
- Increasing speed does not increase the smelting animation speed, but does increase the amount smelted at once
- Increasing speed only works using whole numbers, like 2.0, 3.0, 4.0 -- This means you cannot achieve speeds like 0.5 or 1.5
- The "Smelting Frequencies" config has no effect
- When the plugin loads, if the config has invalid item short names or invalid entity short names, warnings will be printed in the server console to help you learn about the problem as early as possible

## FAQ

If you have a question not listed here, please check the open/closed issues on this GitHub repo. If you do not see your question there, open a new issue.

#### Why do I get a compilation error?

If the error mentions `CanBeCookedByAtTemperature`, you are using a version without the fix in this fork. Use the `QuickSmelt.cs` from this repo.

Otherwise, you may have downloaded the plugin file wrong. Try viewing the cs file in **raw** format on GitHub, then save that as QuickSmelt.cs.

#### Do I need to reset my config to use this fork?

No, existing configs will work.

#### Why is smelting speed having no effect?

It probably is working, and you just don't realize. Adjusting smelting speed does **not** affect the speed of the green progress bar. Instead, the speed options affect the amount of each stack that is smelted when the progress bar completes.

#### Can I suggest new features?

This fork only exists to keep the plugin compiling on newer Rust builds. Feature requests should go to the [original fork](https://github.com/WheteThunger/QuickSmelt), but feel free to report bugs related to the compatibility fix here.

#### How can I speed up industrial conveyors to keep up with the fast smelting speed and high stack sizes?

Try using the Conveyor Stacks plugin and/or adjusting conveyor ConVars.

#### When will this be available on uMod?

The original fork's author plans to contribute some fixes to the uMod version. The compatibility fix in this fork may be offered upstream as well.

## Furnace short names

The following prefab short names are valid in the config.

- `bbq.campermodule`
- `bbq.deployed`
- `bbq.static_hidden`
- `bbq.static`
- `campfire_static`
- `campfire`
- `carvable.pumpkin`
- `chineselantern_white.deployed`
- `chineselantern.deployed`
- `cursedcauldron.deployed`
- `electricfurnace.deployed`
- `fireplace.deployed`
- `furnace_static`
- `furnace.large`
- `furnace`
- `hobobarrel_static`
- `hobobarrel.deployed`
- `jackolantern.angry`
- `jackolantern.happy`
- `lantern.deployed`
- `legacy_furnace`
- `refinery_small_deployed`
- `skull_fire_pit`
- `small_refinery_static`
- `tunalight.deployed`
- `cookingworkbench`
- `industrial.electric.furnace`
- `industrial.furnace.large`
- `apartment_large_furnace`
