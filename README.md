# Doodle Pop 3D 🖍️➡️🧸

A tiny phone web app that turns a kid's drawing into a 3D-printable model for a Bambu Lab A1.

1. **📷 Take a picture** of the drawing (or pick one from the photo library).
2. The drawing becomes a **3D model**: the lines stand up and sit on a base plate.
3. **Edit it** with fingers or by voice:
   - 👆 **Spin**: drag to rotate, pinch to zoom
   - ✏️ **Draw** / 🧽 **Erase**: paint lines onto the model or rub them out
   - 🎤 **Talk**: "bigger", "smaller", "taller", "flatter", "chunky lines", "fill it in",
     "just the outline", "remove the base", "make it blue", "spin", "undo", "start over", "print it"
   - 🎛️ **Change**: sliders for size, height, line thickness and how much faint pencil to pick up
4. **🖨️ Print it!** saves a **3MF** (or STL). It's in millimeters, lying flat and centered on the A1's 256 mm bed.

Everything runs on the phone. No photos are uploaded anywhere.

## Put it on your phone

The camera and microphone need the page served over HTTPS. GitHub Pages is the easy way:

1. On GitHub, open **Settings → Pages** for this repo.
2. Under **Build and deployment**, choose **Deploy from a branch**, then pick the branch and `/ (root)`.
3. Open `https://<your-username>.github.io/kid-prints/` on your phone.
4. Use **Share → Add to Home Screen** (iPhone) or **⋮ → Add to Home screen** (Android) so it opens like an app.

(GitHub Pages on a private repo needs a paid GitHub plan. Otherwise, make the repo public.)

## Getting it onto the A1

- Tap **Print it! → Save 3MF**, and the phone's share sheet opens.
- **Most reliable path:** send the file to a computer (AirDrop, email, Drive), open it in **Bambu Studio**,
  slice it, and send it to the A1.
- **Phone only:** try sharing the 3MF to **Bambu Handy**. Whether Handy can slice a raw model file
  depends on your Handy version. If it won't open it, upload the 3MF to MakerWorld as a private draft
  and print it from Handy from there.
- Suggested settings: PLA, 0.2 mm layers, 15% infill, no supports.

## Tips for good results

- Use a dark marker or crayon on white paper. Thin pencil works too: raise "Pick up light lines".
- Fill the photo with the drawing, and avoid strong shadows across the page.
- Keep **Base plate** on if parts of the drawing aren't connected. Otherwise they print as separate pieces.
- Lines thinner than "normal" may be too fragile to print well.

## How it works

`index.html` is the whole app: plain HTML/JS plus [three.js](https://threejs.org) from a CDN, with no build step.

- **Photo → drawing:** the photo is normalized against a per-tile estimate of the paper's brightness
  (which cancels shadows), then thresholded, cropped to the drawing, and sampled onto a 160-cell grid.
- **Drawing → 3D:** each grid cell gets a height (line, filled area or base). That height map becomes a
  watertight, manifold mesh. Walls are split at every height level and corner-only contacts are removed,
  so slicers don't need to repair anything.
- **Export:** binary STL, and a 3MF (a zip containing the 3MF core model) built in the browser.
