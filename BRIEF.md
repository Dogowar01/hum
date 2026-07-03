# Hum — Claude Code Build Brief

**Product:** Signal9 Apps · *Hum*
**What it is:** A single-file PWA that listens to a room, reads its background level, and fingerprints steady tones (mains hum, whine, rumble), visualised as a reactive 3D sphere whose colour tracks frequency and whose pulse tracks level.

## Current state
A working prototype exists as `index.html` (the reactive sphere, Web Audio analysis, and fingerprint logic all function). **Do not rebuild it — package it.** The remaining work is turning this single file into a properly installable, offline-capable PWA and deploying it.

## Stack constraints (Signal9 house rules — do not violate)
- Single-file HTML app, vanilla JS, **no build step**, no framework.
- No backend. No runtime AI calls.
- Deploy target: **GitHub Pages** (https, required for mic access).
- Primary test device: iPhone (Safari). Windows dev machine.

## Definition of done
1. **Vendor all external dependencies locally** so the app works fully offline:
   - Download `three.min.js` (r128) into `/vendor/` and change the `<script src>` to the local path. (Currently loaded from cdnjs — offline would lose the sphere.)
   - Self-host **Fraunces** and **JetBrains Mono** as `woff2` in `/fonts/`, replace the Google Fonts `<link>` with an `@font-face` block. Keep the same weights in use (Fraunces 300/400/500; JetBrains Mono 300/400/500).
2. **`manifest.json`**: `name: "Hum"`, `short_name: "Hum"`, `display: "standalone"`, `background_color: "#0B0D12"`, `theme_color: "#0B0D12"`, `orientation: "portrait"`, icon set below. Link it in the head.
3. **Service worker** (`sw.js`): cache-first for the app shell (index.html, three.min.js, fonts, icons). Register it in index.html with a graceful no-op if unsupported. Bump a `CACHE_VERSION` const on each deploy.
4. **Icons**: generate a Signal9-style icon set — a warm glowing sphere on the `#0B0D12` ground — at 192, 512, and a 512 maskable, plus a 180 `apple-touch-icon`. Add `apple-touch-icon` link + the existing `apple-mobile-web-app-*` meta are already present.
5. **Deploy**: `git init`, push to a new GitHub repo, enable Pages (main branch, root). Confirm the deployed URL loads over https and the mic prompt fires on iPhone.

## Locked decisions — do NOT change these
- **Honest measurement.** The mic MUST stay opened with `echoCancellation: false, autoGainControl: false, noiseSuppression: false`. Level is reported as **relative dBFS**, never as calibrated SPL. Do not add a fake decibel-meter claim.
- **Fingerprint restraint.** The verdict cell only asserts "Mains hum ~50/60 Hz" or "Steady tone ~X Hz" when the prominence/stability thresholds are met; otherwise "No steady hum." Keep it conservative — no confidently-wrong output.
- **Aesthetic.** Deep near-black ground; the *sphere carries the colour* (warm = low freq, cool = high). Fraunces for the name, JetBrains Mono for all data. Do not switch to the warm-paper Signal9 default — this is a night instrument.
- **No gamification, no streaks, no engagement mechanics.** Quiet instrument.
- Respect `prefers-reduced-motion` (already wired — keep it).

## Out of scope for this pass (future, not now)
- Named-culprit labelling (fridge compressor ~100 Hz, LED/transformer whine, HVAC rumble).
- A "quieter vs. more reactive" visual toggle.
- Session logging / history.

## Notes
- Mic access requires a user gesture (the "Begin listening" button already provides it) and https (Pages provides it). The built-in "Preview without mic" fallback should remain for environments that block the mic.
- Keep everything in one repo, flat where possible: `index.html`, `sw.js`, `manifest.json`, `/vendor/`, `/fonts/`, `/icons/`.
