# Source integration plan

This checklist intentionally avoids guessed symbols. Before editing the runtime, locate the real implementations in this repository and wire the policy to those APIs.

## 1. iOS lifecycle and memory pressure
- Find the UIKit/Swift application lifecycle bridge.
- Handle memory warnings and background transitions.
- On pressure: stop asset prefetch, evict recreatable caches, release temporary decode buffers, and avoid destroying live resources still referenced by a frame.
- Preserve the increased-memory-limit entitlement required by the current README. Verify the entitlement in the signed app.

## 2. Wasmtime / game runtime
- Measure linear-memory growth and peak usage under a real game session.
- Avoid unbounded memory growth; choose limits only after measuring actual game requirements.
- Do not set an arbitrary tiny Wasm memory maximum: it can cause traps/crashes or prevent loading.
- Keep save/progress state safe across suspension and memory-pressure recovery.

## 3. wgpu / Metal renderer
- Inventory actual texture, buffer, render-target, and pipeline allocations.
- Avoid duplicate CPU/GPU copies where ownership permits.
- Bound texture and transient render-target caches.
- Recreate device-dependent resources after a real device-loss/restore event.
- Validate ASTC support and all fallback formats on each target GPU; never assume a format is supported.
- Apply dynamic resolution only through the existing render-size path and test UI scaling.

## 4. Asset pipeline
- Stream world assets in bounded batches rather than decoding the entire world at once.
- Release decode staging memory after upload completes.
- Keep compressed source data where possible; avoid retaining both full decoded and compressed copies without a reason.
- Use distance/LOD selection before expensive decode/upload work.

## 5. Threading and audio
- Measure worker count and peak temporary memory; do not blindly force a fixed thread count.
- Keep audio buffers bounded and avoid allocating large buffers per frame.
- Suspend nonessential work when app is backgrounded or thermally constrained.

## 6. Integration gate
The profile is not considered implemented until every setting has a real code consumer, unit/integration coverage, and device measurements. Unknown TOML keys must not be silently treated as working optimizations.
