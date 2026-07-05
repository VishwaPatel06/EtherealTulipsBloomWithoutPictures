IF YOU CAN'T BUT HER FLOWERS, MAKE HER FLOWERS ;)

# 🌷 Tulip Bouquet — Grow & Bloom Hand Tracker

A real-time computer vision project that turns your webcam into a magic garden. Pinch your thumb and index finger apart on two different hands to **grow** a bouquet of stems and **bloom** vibrant, glowing, golden-bordered tulips — with a pseudo-3D spinning effect and drifting magic sparkles.

Built with **OpenCV** + **MediaPipe Hands**, running entirely in a Jupyter Notebook. No image assets, no external libraries for background removal — every flower is drawn procedurally as glowing vector graphics.

---

## Features

- **Two-hand control** — one hand controls stem **growth** (height), the other controls **bloom** (how open the petals are)
- **Thumb–index pinch tracking** — distance between thumb tip and index tip drives each value, visualized with a live line + label on screen
- **5-stem fan bouquet** — each stem in a different vibrant color (deep red, hot pink, magenta, coral, rose)
- **Pseudo-3D spin** — tulips continuously rotate with a squash-based illusion, each at its own speed and phase
- **Golden petal borders** — every flower is outlined in glowing gold for a polished, jewel-like look
- **Glow/bloom rendering** — additive blur-and-blend pipeline gives flowers a luminous, semi-translucent appearance
- **Magic sparkle particles** — twinkling dust drifts upward near full bloom
- **Single-hand fallback** — works with just one hand if a second isn't detected
- **Live FPS + value readout** for debugging and tuning

---

## 🧰 Requirements

- Python 3.9+
- A working webcam
- Jupyter Notebook / JupyterLab

### Dependencies

```bash
pip install opencv-python mediapipe numpy
```

No image files, no `rembg`, no `Pillow` needed — everything is drawn in code.

---

## 📁 Project Structure

This project is a single Jupyter Notebook with the following cells, run top to bottom:

| Cell | Purpose |
|------|---------|
| 1 | Install dependencies |
| 2 | Imports & MediaPipe Hands setup |
| 3 | Vector tulip shape — colors, petal geometry, 3D spin, golden borders |
| 4 | Thumb–index pinch distance metric |
| 5 | Magic sparkle particle system |
| 6 | Bouquet renderer (stems + tulips, grow/bloom logic) |
| 7 | Main real-time webcam loop |
| 8 | Run function |

---

## ▶️ How to Run

1. Open the notebook in Jupyter.
2. Run all cells in order (Cell 1 → Cell 8).
3. A window titled **"Tulip Bouquet - Grow & Bloom"** will open showing your webcam feed.
4. Show **two hands** to the camera:
   - The hand on the **left** side of the frame controls **Bloom**.
   - The hand on the **right** side of the frame controls **Grow**.
5. Pinch your thumb and index finger together, then spread them apart — watch the bouquet grow and bloom in real time.
6. Press **`q`** in the video window to quit.

> With only one hand visible, that hand controls both Grow and Bloom simultaneously.

---

## 🎛️ Tuning Guide

All adjustable in the notebook, no code restructuring needed:

| What you want to change | Where | How |
|---|---|---|
| Number of stems | Cell 6 → `get_stem_configs(n=5)` | Change `n` |
| Bouquet position | Cell 7 → `anchor = (int(w * 0.25), int(h * 0.85))` | Adjust the `0.25` / `0.85` multipliers |
| Flower size | Cell 6 → `draw_tulip_glow(..., size=110, ...)` | Increase/decrease `size` |
| Spin speed | Cell 6 → `'spin_speed': random.uniform(40, 80)` | Raise for faster spin, lower for slower |
| Flower colors | Cell 3 → `TULIP_PALETTES` | Add/edit BGR color tuples |
| Golden border color | Cell 3 → `GOLD = (25, 215, 255)` | Change BGR values (e.g. deeper amber) |
| Border thickness | Cell 3 → `cv2.polylines(..., 2, ...)` | Increase to 3–4 for a bolder outline |
| Animation smoothness | Cell 8 → `smoothing=0.15` | Lower = slower/dreamier, higher = snappier |
| Glow brightness | Cell 8 → `glow_intensity=1.0` | Raise for a more neon look |
| Pinch sensitivity | Cell 4 → `progress = (pinch_dist - 0.2) / (1.6 - 0.2)` | Raise `0.2` if it triggers too early; lower `1.6` if full spread doesn't reach 1.0 |
| Sparkle density | Cell 7 → `if bloom_smoothed > 0.85 and random.random() < 0.4` | Raise `0.4` for denser sparkles |

---

## 🛠️ How It Works (Technical Overview)

1. **Hand detection** — MediaPipe Hands returns 21 landmarks per detected hand each frame.
2. **Pinch metric** — the distance between landmark `4` (thumb tip) and landmark `8` (index tip) is normalized by hand size (wrist-to-middle-knuckle distance) so it works regardless of how close/far your hand is from the camera.
3. **Role assignment** — detected hands are sorted by horizontal screen position; leftmost = Bloom hand, rightmost = Grow hand.
4. **Smoothing** — an exponential moving average smooths both values frame-to-frame to avoid jittery animation.
5. **Procedural tulip rendering** — each tulip is built from 3 overlapping polygon "petals" (`cv2.fillPoly`), scaled by bloom progress and outlined in gold (`cv2.polylines`).
6. **Pseudo-3D spin** — petal x-coordinates are scaled by `cos(rotation_angle)` each frame, simulating a flower turning in place; shading darkens at "edge-on" angles for added depth.
7. **Glow pipeline** — all shapes are drawn onto a separate float32 layer, blurred twice (sharp + soft pass via `cv2.GaussianBlur`), then additively blended onto the camera frame — this produces the luminous, translucent look rather than flat cartoon fills.
8. **Sparkles** — small particles spawn near full bloom, drift upward, twinkle, and fade out, rendered on the same glow layer.

---

## 🐛 Troubleshooting

- **`SyntaxError` after pasting code** — usually caused by pasting new code into a cell that still has old code in it. Always select-all (`Ctrl+A`) and delete a cell's contents fully before pasting a replacement.
- **`cv2.line` / `cv2.circle` type errors** — coordinates must be Python `int`, not raw floats from MediaPipe landmarks. Make sure any point passed to a drawing function is cast with `int(...)`.
- **Camera doesn't open** — try changing `camera_index=0` to `1` or `2` in the `run_tulip_bouquet(...)` call if you have multiple cameras.
- **Grow/Bloom feels reversed or unresponsive** — recalibrate the pinch thresholds in Cell 4 (see Tuning Guide above).
- **Low FPS** — reduce `cap.set(cv2.CAP_PROP_FRAME_WIDTH/HEIGHT, ...)` resolution in Cell 7, or reduce `max_num_hands` in Cell 2 if only one hand is needed.

---

## 🔮 Possible Extensions

- Soft outer glow ring around each fully-bloomed tulip
- One-time sparkle "burst" on first reaching full bloom instead of continuous drift
- Save/record output video with `cv2.VideoWriter`
- On-screen color picker to let the user choose bouquet palette live
- Wind-sway animation for stems when idle

---

## 📄 License

Personal / educational project — free to use, modify, and extend.
