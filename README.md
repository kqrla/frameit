# frameit

a figma plugin and webapp for wrapping your art in something worth looking at.

---

## what it is

frameit is a creative framing tool — part figma plugin, part standalone webapp — built around one idea: the *frame* is part of the art.

you bring the content. frameit brings the context.

think canva's frames feature, but built for figma-native workflows, with a proper gallery, vector output, optional code export, and a library of frame types that feel like physical objects — things with texture, animation, and personality.

---

## the ux model

### in the plugin (figma)

1. open frameit from the figma plugin menu
2. a **preview panel** appears — browse the frame gallery, hover to preview, right-click to swap types or variants
3. drop your selected layer into a frame with one click
4. customize directly in the panel: animation state, color, wear level, label text, case type, etc.
5. optionally export the frame as a self-contained figma component
6. optionally copy the html/css/scss to your clipboard

the preview screen always comes first — you never blindly dump an image into a frame. you see it, tune it, then confirm.

### in the webapp

1. visit frameit.app
2. upload an image or start from a blank template
3. browse the gallery and preview frames around your image in real time
4. use creation tools to build more complex compositions (postcards, strips, etc.)
5. export as png, svg, animated gif, or html/css snippet

---

## frame catalog

### disc formats

#### cd — circular image crop
- **spinning cd** — image rotates continuously, shine overlay, hollow center hole
  - animation: `spin 3s infinite linear`
  - states: spinning / paused / idle
- **paused cd** — static disc, full styling, no animation
- **cd in jewel case** — album cover sits in front, disc slides out on hover/interaction
  - cover is the main artwork; disc slides right and reveals the cd behind it
  - case can be open or closed state (figma prototype toggle)
  - spine text, back tray card panel, barcode sticker area
- **jewel case only (open)** — just the case, no disc, artwork fills the tray interior
- **digipak** — soft-fold cardboard sleeve style, no plastic case

#### vinyl record
- **12" lp** — standard black vinyl, center label area (image or text), spinning animation
- **7" single** — smaller, center hole is larger relative to disc
- **picture disc** — full-bleed image printed on the vinyl surface
- **colored vinyl** — translucent colored disc variants (red, blue, green, smoke, splatter)
- **vinyl in sleeve** — artwork sleeve front, vinyl peeks out from top or side
- **spinning vinyl** — `spin` animation same as cd variant, label area stays readable if slow enough

#### cassette tape
- **standard cassette** — full cassette body, two spools visible through window, label area
- **c-shell (clear case)** — transparent housing over the cassette
- **tape spools spinning** — spool animation tied to "playing" state
- **lo-fi cassette** — aged, worn label, wrinkled edges, hand-labeled style
- **mixtape cassette** — blank label with custom text and marker-style fonts

#### mp3 player / ipod styles
- **classic ipod** — white clickwheel body, small screen area for your image
- **ipod mini** — aluminum body in color variants (pink, green, blue, silver, gold)
- **ipod nano (1st gen)** — ultra-thin aluminum body
- **generic mp3 player** — mid-2000s style, screen + side buttons
- **ipod touch** — modern touchscreen style, round corners, home button
- all have: screen area (your image fills here), clickwheel or buttons as decoration, optional earphone cable illustration

---

### photo & print formats

#### polaroid
- **standard polaroid** — white border, thick bottom strip, optional handwritten caption
- **mini polaroid** — smaller format, same structure
- **aged polaroid** — yellowed, faded, slight warp at corners
- **color-tinted border** — border takes a color from the image or custom picker
- **string hung** — polaroid with a tiny clothespin and string illustration

#### photo strip
- **4-up vertical strip** — classic booth style, 4 frames stacked
- **3-up strip** — 3 frames, slightly wider proportion
- **custom layout** — variable number of frames, configurable grid
- strip border color, background color, footer text area (date, custom)

#### film strip
- **35mm negative strip** — perforated edges (sprocket holes), frame numbers, orange border
- **color film** — standard c-41 color negative look
- **b&w film** — desaturated look, silver-grain texture option
- **slide (mounted)** — individual slide in a mount frame, transparent center

#### photo frame (classic)
- **thin modern** — 1px–4px border, minimal
- **gallery frame** — thin black or white mat + frame
- **ornate gold** — decorative molding illustration
- **wooden frame** — warm-toned, natural wood texture
- **clip frame** — just binder clips at corners, no border

---

### ephemera & stationery

#### postcard
- **front face** — image area, optional caption strip, decorative border options
- **back face** — divided layout: message area, address lines, stamp box area
- **vintage postcard** — aged paper texture, sepia tone options, serif font defaults
- **holiday / seasonal variants** — christmas, new year, summer, etc.
- full two-sided editor in the webapp

#### rubber stamp
- **circular stamp** — text curves around the outside, icon in center
- **rectangular stamp** — bold border, text in rows, dated format option
- **distress level** — slider from clean to heavily worn (affects ink coverage)
- **ink color** — red, blue, black, purple, custom
- **"received" stamp** — prefab date-received style

#### wax seal
- **round seal** — pressed wax with embossed design in center
- **wax color picker** — red, burgundy, navy, black, gold, ivory, custom
- **monogram seal** — single or double letter inset
- **icon seal** — small icon presets (star, crown, flower, moon, etc.)
- **seal + envelope flap** — seal placed on a folded envelope edge illustration

---

## animation states

every animated frame type (cd, vinyl, cassette, ipod) supports three states:

| state | behavior |
|---|---|
| `spinning` | continuous rotation, full speed |
| `paused` | frozen mid-rotation (no snap to 0) |
| `idle` | stopped at 0°, clean rest position |

in the figma plugin, states map to prototype interactions (click to play/pause toggle).
in the webapp, states are toggled via class on the container element.

```scss
// cd / vinyl base spin
@keyframes spin {
  to { transform: rotate(360deg); }
}

// cassette spool spin (smaller, faster)
@keyframes spool {
  to { transform: rotate(360deg); }
}

// the paused state is just animation-play-state: paused
// no snap — it freezes wherever it is
.frame--paused img {
  animation-play-state: paused;
}
```

---

## jewel case hover — the slide mechanic

one of the signature interactions: the cd lives behind the album cover inside a jewel case. on hover (or prototype trigger in figma), the cover shifts left and the disc slides out to the right.

```scss
.album-wrapper {
  position: relative;

  .thumbs-album {
    // the cover art
    position: relative;
    z-index: 10;
    transition: all 0.3s;
  }

  .compact-disc {
    // the cd behind the cover
    position: absolute;
    margin-left: 120px;
    transition: all 0.3s;
    border-radius: 50%;
    background-size: cover;

    .inner {
      // the center hole
      background: black;
      border-radius: 50%;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);

      .ring {
        // the metallic ring
        background: linear-gradient(to right, #fff 0%, #f1f1f1 50%, #e1e1e1 51%, #f6f6f6 100%);
        border-radius: 50%;
      }
    }
  }

  &:hover .thumbs-album { margin-left: 0; }
  &:hover .compact-disc { margin-left: 150px; }
}
```

figma version: cover and disc are two separate layers; prototype smart animate handles the slide.

---

## creation tools (webapp)

drag-and-drop builders for assembling compositions from scratch:

- **postcard builder** — choose front or back, drag photos into zones, place stamps, add postmark, type message in handwriting font
- **photo strip builder** — arrange photos in strip layout, pick filter presets, add footer text
- **cassette label designer** — fill in track list, choose font, set side a / side b
- **wax seal maker** — monogram picker, color picker, wax texture picker
- **rubber stamp designer** — text input, shape (circle/rect), font, distress slider, ink color
- **cd/vinyl label designer** — circular text layout, center image, color zones

---

## controls — the figma panel

simple and opinionated. no design knowledge required.

every frame has at most:

- **swap frame** — right-click any frame to pick a different type or variant
- **image** — drag in or link any image
- **state toggle** — spinning / paused / idle
- **color** — where applicable (vinyl color, wax color, stamp ink)
- **wear** — a single slider from pristine to heavily used
- **text** — label, caption, spine text, etc. where applicable
- **export** — png / svg / css snippet

---

## tech stack (planned)

| layer | tool |
|---|---|
| figma plugin | figma plugin api + typescript |
| plugin ui | preact + scss modules (tiny bundle) |
| webapp | react + scss modules |
| animations | css keyframes via scss |
| vector output | svg generation / figma vector api |
| frame definitions | json-based frame registry (each frame is a spec file) |
| gallery / cms | tbd — json registry or contentful |
| backend (webapp) | edge functions (cloudflare workers or vercel) |
| export (code) | handlebars templates → html/css/scss output |

---

## frame definition format (draft)

each frame is described by a json spec that the gallery reads:

```json
{
  "id": "cd-jewel-spinning",
  "label": "cd — jewel case (spinning)",
  "category": "disc",
  "tags": ["cd", "jewel case", "spinning", "animated"],
  "states": ["spinning", "paused", "idle"],
  "slots": {
    "cover_image": { "type": "image", "required": true },
    "disc_image": { "type": "image", "required": false, "fallback": "cover_image" }
  },
  "controls": {
    "state": { "type": "toggle", "options": ["spinning", "paused", "idle"] },
    "spin_speed": { "type": "range", "min": 1, "max": 10, "default": 3, "unit": "s" },
    "shine": { "type": "boolean", "default": true }
  },
  "exports": ["png", "svg", "css"]
}
```

---

## status

early concept. open for ideas, frame suggestions, and contributions.

if you know a frame type that should be here — open an issue.

---

## made by

karla — [github.com/kqrla](https://github.com/kqrla)
