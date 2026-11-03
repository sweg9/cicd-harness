---
name: build-sim-demo
description: Build the sim-demo Unity project for macOS and Windows and package the results as zip files. Use when asked to build, package or release the simulator demo, or when CI needs the player builds.
---

# Build sim-demo

Source repo: https://github.com/sweg9/sim-demo (Unity 6000.5.4f1, built-in render pipeline, Mono).

## Steps

1. Set `UNITY` to the Unity 6000.5.4f1 editor binary and `PROJ` to the sim-demo checkout.
2. Close any Unity editor that has the project open. A second instance on the same project fails.
3. Regenerate materials and the scene, as its own run:
   `"$UNITY" -batchmode -quit -projectPath "$PROJ" -executeMethod Setup.Run -logFile setup.log`
4. Build macOS: `... -executeMethod Setup.Build -logFile build-mac.log` writes `Builds/CLOiD-Demo.app`.
5. Build Windows: `... -executeMethod Setup.BuildWin -logFile build-win.log` writes `Builds/Windows/CLOiD-Demo.exe`.
6. Package. macOS needs `ditto -c -k --sequesterRsrc --keepParent Builds/CLOiD-Demo.app Builds/sim-demo-macOS.zip`. Windows zips the `Builds/Windows` folder.
7. Report the last `BUILD Succeeded` line and the zip sizes from each log. Nothing else.

## Rules

- Never call `Setup.Run` inside the build step. Adding shaders to GraphicsSettings during a build caused "Failed to write file ... unity_builtin_extra".
- `-nographics` cannot render, so it is fine for builds and not for screenshots.
- Cap log size, for example `ulimit -f 200000` in the shell, because a per-frame error can fill the disk.
- The macOS app is unsigned. Say so in the report.
- Do not claim the build was run or tested unless a player was actually started.
