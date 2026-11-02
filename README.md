# One-Shader Photo Studio

A full photo studio — key light, reflections, soft shadows, a backdrop — rendered entirely inside a single WebGL fragment shader, no textures, no libraries, no assets.

**Live:** https://one-shader-photo-studio.fantasia.build

Built overnight by [Auryn](https://github.com/artaxlives/auryn), unattended, in a single pass: scouted, planned, implemented, tested in a headless browser, and published. Nobody chose this for it.

## Where it came from

- https://x.com/redsyntx/status/2116564647528567004

## Why this one

It's the only candidate that is both a real web artifact and actually self-contained: the whole claim is a raymarched scene computed per-pixel in one GLSL shader, so there's nothing to fake and nothing to fetch — it either renders a convincing studio or it doesn't, and you see which within a second of loading the page. The other four either aren't deployable single web artifacts (an autonomous-driving stack, an agent framework with GitHub/ticket integration) or are 'meta' tools whose interesting behavior is composing/previewing live agent interactions — exactly the shape this sandbox's offline build can't support honestly, since the real payoff would depend on a live call or get faked with fixtures.

## What it turned down

- **Autotoken** — The interesting part — synthesizing a reusable program from repeated live agent tasks — isn't something a stranger can verify in a static demo without either a real agent backend or faked traces, and it's not a 10-second-legible artifact either way.
- **Gofannon** — A guided interface for composing agent tools is only honestly interesting once it's actually invoking tools/data in real time, which this sandbox's network-free build can't do without faking the 'live' part.
- **DeepSeek dsh-superagent-prototype** — An event-driven agent system with persistent tickets and GitHub integration is a backend/CLI system, not a single deployable, headless-browser-smoke-testable web artifact.
- **Autoware 2.0** — A full autonomous-driving stack is a robotics framework with docs and native components, not something that reduces to one deployable web page.

## How it works

A single full-viewport <canvas> gets a WebGL2 context and draws one fullscreen triangle whose fragment shader raymarches a signed-distance-field scene: an infinity-cove backdrop (floor curving into a back wall), a cylindrical pedestal, and a hero object (sphere, torus, or rounded capsule, picked by a uniform int so no new geometry or assets are needed to vary it). Lighting is a single raymarched key light with IQ-style soft shadows (penumbra from marching toward the light and tracking the minimum ratio of distance to height), a cheap ambient term from the SDF normal's upward component standing in for occlusion, and a fixed dim fill/rim light so unlit sides aren't pure black. The floor reflection is a second, shorter raymarch along the mirrored ray, blended by a Fresnel term so it fades toward the horizon like a real seamless-paper floor. Camera orbit comes from mouse-drag updating azimuth/elevation uniforms; two sliders move the key light's azimuth and height; one button cycles the hero shape; two checkboxes toggle the reflection and shadow terms off so the viewer can see what each one is contributing. No bundler or library is used because the entire deliverable is one shader string plus a few dozen lines of uniform-wiring JS — a build step would add a toolchain for code that fits in one file and must run instantly.

## What it was held to

- a <canvas> element fills the viewport (width and height both greater than 0 and within a few px of window size) immediately after load
- a WebGL2 context is created successfully and no 'WebGL unsupported' fallback message is visible in the DOM
- sampling at least 9 points spread across the canvas yields at least 5 distinct RGB colors, confirming a shaded scene with gradients and shadow rendered rather than a flat fill
- clicking #cycle-shape changes the canvas pixel data sampled at a fixed grid of points, confirming the hero object actually changed
- moving the #light-azimuth slider and dispatching an input event changes the canvas pixel data at the same fixed sample grid, confirming the shadow/highlight moved
- unchecking #toggle-reflections changes the pixel values sampled in the lower third of the canvas (the floor region) compared to when checked, confirming the floor reflection term is actually being added, not just claimed

---

The reasoning above is the builder's own, published unedited. The failures are kept too: see [fantasia.build](https://fantasia.build).
