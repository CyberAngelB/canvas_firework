# Canvas Fireworks

An interactive fireworks show in a single HTML file. Click anywhere and a rocket
arcs up from the bottom of the screen and bursts exactly where you clicked,
scattering glowing particles that drift, fall, and fade against a starry night sky.

## Run it

No build step and no dependencies — just open `index.html` in any modern browser.
Optionally serve the folder (nicer for mobile testing):

```sh
python -m http.server 8123
```

then visit <http://localhost:8123>.

## Controls

| Action | Result |
| --- | --- |
| Click anywhere | Launch a firework that bursts at that spot |
| Click rapidly | As many fireworks at once as you like — no cooldown |
| Hold and drag | Stream fireworks along the cursor path |
| Space / Enter | Launch one at a random spot |

## Features

- **Six burst types, picked at random per launch:** peony (hollow or filled shell),
  tilted ring, two-tone double burst, drooping gold willow, dense fill, and
  crackle — whose sparks pop into little white flashes near the end of their life.
- **Ballistic rockets** that fly a real arc from the bottom of the screen and
  explode right at the click point, trailing gold sparks on the way up.
- **Additive glow:** particles are drawn as velocity-streaked sparks with
  `lighter` blending, so overlaps bloom. A white-hot flash — with the occasional
  expanding shockwave ripple — punctuates each explosion.
- **Comet trails:** each frame fades toward black instead of erasing, leaving
  glowing trails behind every particle.
- **Performance-minded:** delta-time physics (smooth on any refresh rate),
  device-pixel-ratio-aware rendering (capped at 2×), and a 3,200-particle budget
  that quietly shrinks burst sizes under heavy spam instead of dropping frames.
- **Little extras:** a short opening show on load, gentle ambient fireworks when
  idle (skipped when the OS asks for reduced motion), touch support, and a hint
  line that fades away after your first launch.

## How the trails work (implementation note)

The particle canvas is opaque black and blended onto the page with
`mix-blend-mode: screen`, so its black background is neutral and the starfield
layer beneath shows through. Every frame paints a translucent black rectangle
over the previous one, decaying old pixels toward black — which is invisible
under screen blending — rather than erasing them. That decay is what produces
the trails, and any rounding residue bottoms out at near-black, so the sky never
accumulates ghost artifacts even after minutes of play.
