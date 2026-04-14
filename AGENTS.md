# AGENTS.md — MindAR Multi-Target Demo

> This file is written for AI coding agents. It summarises the project structure, technology stack, conventions, and how to run or modify the demo.

---

## Project Overview

This is a **static, single-page demo** that reproduces the official MindAR multi-target A-Frame example. It runs entirely in the browser and uses the webcam for image-tracked augmented reality.

- **Entry point:** `index.html`
- **No build system:** The project is plain HTML/CSS/JS with dependencies loaded from CDN.
- **Local serve required:** Browsers restrict camera access to secure contexts (`localhost` or HTTPS), so the page must be served with a local web server rather than opened directly from the file system.

---

## Technology Stack

| Layer | Technology | Version / Notes |
|-------|------------|-----------------|
| Base | HTML5 + CSS + vanilla JS | Inline styles and scripts in `index.html` |
| WebXR framework | A-Frame | `1.5.0` loaded from `https://aframe.io/releases/1.5.0/aframe.min.js` |
| Extras | aframe-extras | `7.0.0` loaded from `jsdelivr` (provides `animation-mixer`) |
| AR tracking | MindAR | `1.2.5` loaded from `jsdelivr` (`mindar-image-aframe.prod.js`) |
| Local server | Python `http.server` | Recommended for local development |

---

## Project Structure

```
/Volumes/External/DevExteralHD/MindAR with example
├── index.html                                    # Single demo page (scene, styles, scripts)
├── README.md                                     # Human-facing run instructions & troubleshooting
├── aktasok-salsa-151315.mp3                      # Unreferenced audio file (orphaned)
├── docs/
│   └── plans/
│       ├── 2026-04-01-mindar-multi-target-design.md   # Design record
│       └── 2026-04-01-mindar-multi-target-demo.md     # Implementation plan
```

### Key Files

- **`index.html`** — Contains the full A-Frame scene, two `mindar-image-target` entities, an instruction overlay, status indicator, and all JavaScript event handling for camera start / AR ready / target found / target lost.
- **`README.md`** — Describes how to run the local server, where to find test images, and how to debug camera-permission issues.
- **`docs/plans/*.md`** — Design and implementation planning artifacts. These are read-only historical records.

---

## Runtime Architecture

1. **Page load**
   - A-Frame, aframe-extras, and MindAR scripts load from CDN.
   - The `a-scene` is configured with `mindar-image` pointing to the official remote `.mind` target database.

2. **Scene ready**
   - On the `loaded` event the JavaScript grabs the `mindar-image-system` and enables the **Start Camera** button.
   - Status pill updates to "Ready to start camera".

3. **User interaction**
   - Clicking **Start Camera** calls `arSystem.start()`.
   - On success the `arReady` event fires: the overlay auto-hides, the status turns green, and the video feed becomes visible.
   - On failure the `arError` event fires or the `catch` block updates the status to red and shows troubleshooting text.

4. **Tracking**
   - `targetIndex: 0` — raccoon GLTF model appears when the raccoon image is detected.
   - `targetIndex: 1` — bear GLTF model appears when the bear image is detected.
   - `targetFound` / `targetLost` events update the status text.

### External Assets (Remote)

- **Target database:** `https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/band-example/band.mind`
- **Raccoon model:** `https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/band-example/raccoon/scene.gltf`
- **Bear model:** `https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/band-example/bear/scene.gltf`

> The demo currently **requires an internet connection**. If an offline/self-contained version is needed later, download the `.mind` file and models into an `assets/` folder and update the URLs in `index.html`.

---

## Build and Run Commands

There is **no build step**. To run locally:

```bash
python3 -m http.server 8000 --directory "/Volumes/External/DevExteralHD/MindAR with example"
```

Then open:

- http://127.0.0.1:8000
- http://127.0.0.1:8000/index.html

### Quick Verification (No Camera)

```bash
curl -s http://127.0.0.1:8000/ | head -n 5
```

Expected: the first lines of `index.html` starting with `<!doctype html>`.

---

## Code Style Guidelines

- **Single-file demo:** All markup, styles, and scripts live in `index.html`. If the demo grows, prefer keeping it lightweight rather than introducing a bundler.
- **CSS variables:** The overlay uses a `:root` theming block (`--panel-bg`, `--accent-start`, etc.). Continue using CSS custom properties for any new UI additions.
- **Event-driven JS:** The code relies on A-Frame/MindAR custom events (`loaded`, `renderstart`, `arReady`, `arError`, `targetFound`, `targetLost`). Attach listeners to `sceneEl` or the target entities rather than polling.
- **Accessibility / UX:** Provide clear status text and disable buttons while async operations (camera start) are in flight.

---

## Testing Instructions

There are **no automated tests** in this repository. Validation is manual:

1. **Static serve test** — Start the Python server and `curl` the root. Expect HTTP 200.
2. **Camera / AR test** — Open the page in a browser, click **Start Camera**, allow webcam permissions, and point the camera at the raccoon or bear target image.
   - Raccoon target: https://hiukim.github.io/mind-ar-js-doc/assets/images/raccoon-2ef571baece2ee4724d0d19edf3de791.png
   - Bear target: https://hiukim.github.io/mind-ar-js-doc/assets/images/bear-3c737546fb0bde7a9c45b45ee999d132.png
3. **Troubleshooting camera issues** — Check browser and macOS System Settings camera permissions, close other apps using the webcam, and reload.

---

## Security Considerations

- **Camera access:** The page requests camera permissions via MindAR. There is no video upload or remote streaming; processing happens locally in the browser.
- **Third-party CDNs:** All JavaScript libraries and 3D assets are fetched from public CDNs (`aframe.io`, `jsdelivr`, `github.io`). If you deploy this to a production environment with a strict Content-Security-Policy, allow-list these domains or switch to self-hosted copies.
- **No secrets:** The repository contains no API keys, tokens, or environment variables.

---

## Common Modifications

- **Switch to local assets:** Download `band.mind`, `raccoon/scene.gltf`, and `bear/scene.gltf` into a new `assets/` folder, then replace the CDN URLs in `index.html` with relative paths (e.g., `assets/band.mind`).
- **Add more targets:** The current `.mind` file supports exactly two targets. To add more you must generate a new target database with the MindAR compiler and update the scene accordingly.
- **Change model placement:** Edit the `rotation`, `position`, and `scale` attributes on the `<a-gltf-model>` elements inside each `<a-entity mindar-image-target>`.
