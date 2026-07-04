# frameit

a figma plugin and webapp for wrapping your art in something worth looking at.

---

## what it is

frameit is a creative framing tool — part figma plugin, part standalone webapp — built around the idea that the *frame* is part of the art. you bring the content, frameit brings the context: spinning cds, cracked jewel cases, vinyl records, polaroids, film strips, photo strips, postcards, rubber stamps, wax seals, and more.

think canva's frames feature, but built for figma-native workflows, with vector output, optional code export, and a gallery of deeply customizable frame types that actually feel like something.

---

## the concept

most design tools treat frames as a rectangle with a border. frameit treats them as objects with character — tactile, nostalgic, physical things that carry meaning before you even add your image.

a cd frame isn't just a circle crop. it spins. it catches light. it has a shine overlay, a hollow center, a jewel case you can open or close. a polaroid isn't just a white border — it has aging, grain, a handwritten caption slot, a curl at the corner.

the goal is to make these feel effortless to use but deeply satisfying to customize.

---

## frame types (planned)

- **cd / compact disc** — spinning or paused, with shine overlay and jewel case option
- **vinyl record** — classic black with label area, spinning animation
- **cd cover** — jewel case artwork, spine text, back tray card
- **polaroid** — instant photo frame with caption area and optional aging
- **photo strip** — 4-up vertical strip, booth-style
- **film strip** — 35mm negative border with sprocket holes
- **photo frame** — classic ornate or minimal frame styles
- **postcard** — customizable front/back with stamp area and postmark
- **rubber stamp** — bold circular or rectangular stamp with custom text
- **wax seal** — pressed seal with monogram or icon, color options

---

## the animation layer

for the figma plugin version, frame animations are expressed as figma prototype interactions — no external dependencies. for the webapp version, frames are driven by scss/css animations, with the same visual logic.

example — the cd frame animation:

```scss
.artwork {
  border-radius: 50%;
  overflow: hidden;
  position: relative;

  &:before {
    // shine overlay
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 2;
    background: url(shine.png) center no-repeat;
    background-size: cover;
    mix-blend-mode: overlay;
  }

  &:after {
    // center hole
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    border-radius: 50%;
    box-shadow: inset 0 .2em .2em rgba(0,0,0,.25);
  }

  img {
    display: block;
    width: 100%;
    animation: spin 3s infinite linear;
    border-radius: 50%;
  }
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

each frame type has a `paused`, `playing`, and `idle` state. controls are intuitive — one toggle to spin, one to pause.

---

## the gallery

both the plugin and the webapp include a browsable gallery of frame presets. presets are community-contributed and curated. every preset is:

- fully editable with simple controls (no design knowledge required)
- vector-based where possible
- exportable as png, svg, or css/html snippet

the gallery is the heart of the product — it should feel like browsing a vintage shop, not a dropdown menu.

---

## creation tools (webapp)

beyond framing existing images, the webapp includes drag-and-drop creation tools for:

- **postcards** — blank templates with photo drop zones, stamp placement, postmark overlays, handwritten font options
- **photo strips** — layout builder with filter presets and caption slots
- **wax seals** — monogram builder with wax color picker and stamp shape selector
- **rubber stamps** — text customizer with font, distress level, ink color options
- **cd covers** — full jewel case editor with front, back, and spine panels

---

## figma plugin version

the plugin lives inside figma and works directly on selected frames or images. features:

- apply a frame preset to any selected layer in one click
- customize frame properties in a side panel (color, animation state, wear/aging level)
- prototype-ready — animations are wired to figma prototype interactions automatically
- export frame as a component to reuse across the file
- optional code export: copies the css/html for the framed element to clipboard

---

## webapp version

the webapp is a standalone tool at frameit.app (planned). features:

- upload any image and wrap it in a frame
- browse the gallery and apply presets
- use creation tools to build from scratch
- download as png, svg, or animated gif
- share a link to your framed piece

---

## tech stack (planned)

| layer | tool |
|---|---|
| figma plugin | figma plugin api + vanilla ts |
| webapp ui | react + scss modules |
| animations | css keyframes / scss |
| vector output | svg generation |
| backend (webapp) | tbd — likely edge functions |
| gallery/cms | tbd — possibly contentful or a custom json registry |

---

## status

early concept. repo is open for ideas, contributions, and frame suggestions.

if you have a frame type you want to see — open an issue.

---

## made by

karla — [github.com/kqrla](https://github.com/kqrla)
