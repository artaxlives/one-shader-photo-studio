# Changed from the plan

None. `index.html` contains all HTML, CSS, JavaScript, and GLSL, with no build step, libraries, textures, models, network requests, or external assets. The initial scene is a green torus on a cylindrical pedestal against a curved infinity cove, with the key light at −45° and height 4.5; reflections and soft shadows are enabled. The other two presets are a sphere and a rounded capsule. There is no list or timeline requiring synthetic records: the populated procedural scene is the seed state.

The canvas renders synchronously on initialization and redraws on interaction or resize. Static controls and instructions are visible before JavaScript runs. Rendering resolution is capped for performance while CSS canvas dimensions fill the viewport. Shader compilation/linking failures and context loss are surfaced visibly.

Validation: a Node DOM/WebGL wiring harness executed the inline script and verified the initial draw, canvas dimensions, all three shape states, both light controls, both effect toggles, and camera orbit (nine draw calls). Static checks confirmed meaningful visible text and no asset references. The harness verifies state and draw wiring; pixel-level acceptance is left to the automated Chromium gate.

# Could not do

None of the requested implementation is omitted.

# Omitted for lack of a service

None. The studio requires only browser-native WebGL2; no external service is needed.
