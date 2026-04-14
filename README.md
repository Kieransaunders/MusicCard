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

## Test images

Open these on another device, on another monitor, or print them:

- Raccoon target: `target-raccoon.png` (or https://hiukim.github.io/mind-ar-js-doc/assets/images/raccoon-2ef571baece2ee4724d0d19edf3de791.png)
- Bear target: `target-bear.png` (or https://hiukim.github.io/mind-ar-js-doc/assets/images/bear-3c737546fb0bde7a9c45b45ee999d132.png)

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
