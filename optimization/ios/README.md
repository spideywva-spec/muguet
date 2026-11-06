# Muguet iOS low-memory optimization pack

**Status: experimental integration specification — not yet wired into the game runtime.**

This pack targets investigation of Muguet on memory-constrained iPhones. It does not claim that GTA V will run on a 2 GB device, and it does not silently change renderer or game settings. The upstream README currently recommends an iOS device with 8 GB RAM and requires the increased-memory-limit entitlement.

## What's included

- `profiles/ios-2gb.toml`: proposed memory/performance policy and guardrails for a future runtime integration.
- `INTEGRATION.md`: source-level work required before the profile can affect the app.
- `VALIDATION.md`: repeatable device test matrix and acceptance criteria.

## Important

The TOML file is a **design profile**, not a configuration file currently read by Muguet. No existing engine setting names or APIs were guessed. The profile must be connected to the actual Wasmtime, wgpu, asset streaming, and iOS lifecycle code before it has runtime effect.

A 2 GB device has less than 2 GB available to the game: iOS, SpringBoard, graphics allocations, audio, and other processes also consume memory. The app may be terminated by iOS even when its own allocator appears healthy. Do not remove the increased-memory-limit entitlement as a substitute for optimization.

## Build / package

Open the repository's **Actions → Package iOS optimization pack → Run workflow**. The workflow creates `muguet-ios-optimization-pack.zip` as a downloadable artifact. This pack is separate from the game's copyrighted assets; supply your own legally obtained game files as required by the main README.
