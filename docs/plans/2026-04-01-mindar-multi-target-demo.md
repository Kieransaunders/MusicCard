# MindAR Multi-Target Demo Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a static local MindAR multi-target demo that mirrors the official A-Frame example and can be run from this folder on `localhost`.

**Architecture:** Use one HTML entry point with CDN-loaded dependencies, a full-screen `<a-scene>`, and two anchored GLTF models tied to `targetIndex: 0` and `targetIndex: 1`. Add a small overlay that explains how to run the page and where to get the test images.

**Tech Stack:** HTML, CSS, A-Frame 1.5.0, `aframe-extras` 7.0.0, MindAR 1.2.5, Python `http.server`

---

### Task 1: Capture the selected setup

**Files:**
- Create: `docs/plans/2026-04-01-mindar-multi-target-design.md`
- Create: `docs/plans/2026-04-01-mindar-multi-target-demo.md`

**Step 1: Write the short design record**

Document the chosen static approach, the remote asset assumption, and the local serving requirement.

**Step 2: Write the implementation plan**

Describe the single-page structure, the scene entities, and the verification steps.

### Task 2: Build the static demo page

**Files:**
- Create: `index.html`

**Step 1: Add the documented scripts**

Load A-Frame, `aframe-extras`, and `mindar-image-aframe.prod.js` from CDN.

**Step 2: Define the scene**

Create one `a-scene` that points to the official multi-target `.mind` file and add two `mindar-image-target` anchor entities with separate models.

**Step 3: Add a lightweight instruction overlay**

Explain how to grant camera access and provide direct links to the two target images used by the demo.

### Task 3: Document how to run it

**Files:**
- Create: `README.md`

**Step 1: Describe the local server flow**

Use `python3 -m http.server 8000` so the page runs on `localhost`.

**Step 2: Document test images and constraints**

Explain that the current version depends on remote assets and requires manual camera testing.

### Task 4: Verify the generated demo

**Files:**
- Verify: `index.html`
- Verify: `README.md`

**Step 1: Check the files exist**

Run a file listing command from the project root.

**Step 2: Serve the folder locally**

Run: `python3 -m http.server 8000`

**Step 3: Fetch the page**

Run: `curl http://127.0.0.1:8000/`

Expected: HTTP 200 response with the generated HTML content.

**Step 4: Record the remaining manual test**

Note that a real browser and camera are still needed to fully validate target detection.
