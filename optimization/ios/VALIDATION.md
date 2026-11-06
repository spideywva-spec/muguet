# Low-memory iOS validation

A build succeeding is not proof that the game is stable. Test on physical devices and record exact device model, iOS version, entitlement state, build SHA, game build, and graphics preset.

## Test matrix
1. Clean install and first launch.
2. Main menu idle for 10 minutes.
3. Load a small scene, then a dense/open-world scene.
4. Drive/fly quickly through streamed areas to stress asset churn.
5. Repeat scene load/unload at least 10 times.
6. Background and foreground the app.
7. Trigger an iOS memory warning in a controlled development build.
8. Run for 30 minutes while monitoring thermal state and memory.
9. Repeat with audio and controller connected.
10. Verify suspend/resume and save integrity.

## Record
- Process resident/physical footprint peak and average.
- Wasm linear memory peak and growth events.
- GPU resource/texture memory estimates where available.
- Frame-time p50/p95/p99, not only average FPS.
- Asset decode/upload time and cache hit rate.
- Jetsam reports, crashes, Wasm traps, Metal validation errors.
- Thermal state and battery drain.

## Acceptance criteria
- No reproducible crash, jetsam termination, Wasm trap, or GPU resource leak during the test matrix.
- Memory returns near baseline after repeated scene transitions.
- No unbounded cache growth.
- No visual corruption from texture-format fallback or dynamic resolution.
- 30-minute run completes without progressive slowdown or thermal runaway.
- 30 FPS is a target, not a guarantee; report measured frame times on each device.

A 2 GB physical-RAM device may remain unsupported even after these changes. Publish results per device instead of claiming universal compatibility.
