# Hum

> A quiet instrument for a noisy room.

A single-file Progressive Web App that listens to a room, reads its background level, and fingerprints steady tones — mains hum, a whine, a low rumble — visualised as a reactive 3D sphere whose colour tracks frequency and whose pulse tracks level.

100% offline after first load. No backend, no accounts, no runtime AI calls.

## Features

- **Reactive sphere** — a Three.js icosahedron deformed by simplex noise driven by live mic bands (bass/low-mid/mid/treble), coloured warm→cool by dominant frequency.
- **Honest level readout** — relative dBFS only, mic opened with echo cancellation, AGC and noise suppression disabled. No fake calibrated-SPL claims.
- **Hum fingerprint** — flags "Mains hum ~50/60 Hz" or "Steady tone ~X Hz" only when prominence and stability thresholds are met; otherwise "No steady hum." Conservative by design.
- **Preview without mic** — a synthetic demo mode for environments that block mic access.
- Respects `prefers-reduced-motion`.

## Tech

- Single `index.html` — all CSS and JS inline, zero build step.
- Three.js (r128) vendored locally in `/vendor/`.
- Fraunces + JetBrains Mono self-hosted as variable `woff2` in `/fonts/`.
- Web App Manifest + Service Worker (cache-first app shell) for installability and full offline use.

## Run it

Open `index.html` directly in a browser, or visit the GitHub Pages URL. Install it to your home screen for full offline use. Mic access requires https (Pages provides it) and a user gesture (the "Begin listening" button).

## Disclaimer

Levels shown are relative dBFS, not calibrated sound pressure level. This is a listening instrument, not a certified decibel meter.
