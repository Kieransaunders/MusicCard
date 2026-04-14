# MindAR Multi-Target Demo

This folder contains a static A-Frame + MindAR image-tracking demo based on the official multi-target example.

## Live demo

The site is automatically deployed to Netlify:

**https://mindar-multi-target-demo.netlify.app**

## Markers & QR code

Open the companion page to see the QR code and both target images at a size that's easy to scan. You can also print this page.

**https://mindar-multi-target-demo.netlify.app/markers.html**

The QR code and target images are also saved locally in this repo:

- `qr-code.png` — opens the live demo
- `target-raccoon.png` — Raccoon AR target
- `target-bear.png` — Bear AR target
- `markers.html` — printable companion page with the QR code and both targets

## Official documentation

| Topic | Link |
|-------|------|
| MindAR installation | https://hiukim.github.io/mind-ar-js-doc/installation |
| MindAR examples summary | https://hiukim.github.io/mind-ar-js-doc/examples/summary |
| Multi-targets example | https://hiukim.github.io/mind-ar-js-doc/examples/multi-targets |
| A-Frame `sound` component | https://aframe.io/docs/1.5.0/components/sound.html |

## Run locally

Camera access normally requires `localhost` or another secure context, so serve the folder with a local web server instead of opening `index.html` directly.

```bash
python3 -m http.server 8000 --directory "/Volumes/External/DevExteralHD/MindAR with example"
```

Then open:

- http://127.0.0.1:8000
- http://127.0.0.1:8000/index.html
- http://127.0.0.1:8000/markers.html

Once the page loads, click **Start Camera**.

## How to test

### Quick test (live demo)

1. On your phone, open the live demo: **https://mindar-multi-target-demo.netlify.app**
   - Or scan the `qr-code.png` to jump straight to it.
2. On a second device (or a printed page), open the markers page: **https://mindar-multi-target-demo.netlify.app/markers.html**
3. Tap **Start Camera** on the phone and allow camera access.
4. Point the phone at either the **Raccoon** or **Bear** target image.
5. When tracking locks on:
   - The 3D model appears over the image
   - The background music starts playing
   - The status text updates (e.g. "Raccoon target found")
6. Move the target out of view — the model disappears and the music pauses.

### Local test

1. Serve the folder locally (see [Run locally](#run-locally)).
2. Open `http://127.0.0.1:8000/index.html` on a device with a camera.
3. Open `http://127.0.0.1:8000/markers.html` on another screen, or print the targets from that page.
4. Click **Start Camera** and point the camera at the Raccoon or Bear target.

### Target images

You can also open the raw target images directly:

- Raccoon target: `target-raccoon.png`
- Bear target: `target-bear.png`

## Current setup

- Uses the official hosted MindAR `.mind` file
- Uses the official hosted example GLTF models
- Requires internet access for the external scripts and assets
- Uses a manual **Start Camera** button and status indicator to make camera startup clearer on desktop browsers
- Deployed to Netlify via the Netlify CLI (`netlify deploy --prod --dir .`)

### Audio feature

The demo plays the local MP3 (`aktasok-salsa-151315.mp3`) when an image target is tracked:

- **Preload** — The audio file is registered inside A-Frame `<a-assets>` so the scene blocks until it is cached  
  (see [A-Frame sound docs](https://aframe.io/docs/1.5.0/components/sound.html)).
- **Playback** — An invisible `<a-entity sound="src: #sfx; autoplay: false; loop: true">` is used.
  - Music **starts** on `targetFound` (`playSound()`)
  - Music **pauses** on `targetLost` (`pauseSound()`)
- **Web Audio unlock** — Browsers suspend the Web Audio `AudioContext` until a user gesture.
  The **Start Camera** click resumes the context so subsequent `playSound()` calls work on all targets.

If you want the next step to be a fully offline or self-contained version, the same scene can be switched over to local assets in this folder.

## Troubleshooting

### If you see a directory listing instead

That means the server is pointing at the wrong folder. Use the exact command above with `--directory`, then open `http://127.0.0.1:8000/index.html`.

### If the page asks for camera permission but stays blank

Try these in order:

- Click **Start Camera** after the page finishes loading
- Make sure macOS allows your browser to use the camera in `System Settings > Privacy & Security > Camera`
- Close other apps that may already be using the webcam
- Reload the page after changing permissions
- Use the bear or raccoon image on a separate device or on paper so the Mac webcam can see it

### If audio does not play when a target is found

- Check that the device volume is up and the browser tab is not muted
- On iOS, make sure the hardware silent switch is off (it mutes Web Audio)
- Open the browser console and look for `[AR] ... target found` logs or any sound-component errors
