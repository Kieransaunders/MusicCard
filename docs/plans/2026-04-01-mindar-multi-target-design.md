# MindAR Multi-Target Demo Design

## Goal
Create a simple, runnable demo in this folder that reproduces the official MindAR multi-target A-Frame example and is easy to launch locally.

## Chosen Approach
Use a static `index.html` that loads A-Frame, `aframe-extras`, and MindAR from their documented CDN URLs. Keep the official hosted `.mind` file and GLTF model assets so the first version works quickly in an empty workspace without adding a build tool.

## Architecture
- `index.html` renders the full-screen AR scene and a small instruction panel.
- The A-Frame scene uses one `mindar-image-target` entity per image target.
- The demo keeps the official remote target database and model assets.
- `README.md` documents how to run the page on `localhost`, which is required for camera access.

## Constraints
- Camera permissions usually require `localhost` or another secure context.
- This first pass depends on internet access because scripts, the `.mind` target file, and models are hosted remotely.
- If an offline or self-contained version is needed later, the same scene can be updated to point at local copies under an `assets/` folder.

## Verification
- Confirm the expected files exist.
- Serve the folder locally and fetch the generated page over HTTP.
- Manual camera testing is still required to fully validate AR tracking on a device.
