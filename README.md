# soksak-plugin-shark

A SOKSAK shark mascot that **swims freely across the app window**. The wordmark SOKSAK
is itself a shark (S=head, K=tail fin), and that shark autonomously navigates the entire
window in all directions.
**Swimming liveliness is tied to movement speed (levels 1–10)** — sprinting to a distant
target thrashes the tail vigorously; leisurely cruising flows gently.

## Installation / Activation

```
# Clone the git repository into ~/.soksak/plugins/, or load a local path as dev
sok plugin.dev.load '{"path":"/path/to/soksak-plugin-shark"}'   # dev load
```

Once activated, the shark begins swimming immediately.

## Commands

| Command | Description |
|---|---|
| `plugin.soksak-plugin-shark.toggle` | Show/hide the shark (hiding stops rAF entirely) |
| `plugin.soksak-plugin-shark.mode '{"reactive":true}'` | Reactive mode on/off — when on, the shark subtly dodges the cursor (off=pure decoration, passes through) |
| `plugin.soksak-plugin-shark.speed '{"liveliness":1.5}'` | Liveliness multiplier 0.3–3 (affects overall movement and swimming speed) |

## How It Works

- **Swimming = forward kinematics chain**: The head (SO) is rigid and yaws together at
  the neck (+ subtle S\|O separation), body joint angles (K·S·A) propagate and accumulate
  from head to tail so the tail (K) sweeps the most (posterior-focused thunniform —
  real shark propulsion). Joints are connected and never break.
- **Speed → liveliness**: Current speed is mapped to levels 1–10 to adjust joint angle
  amplitude and swimming frequency. Calm at rest (1–3), thrashing at 8–10 when sprinting
  to a distant target.
- **Swims over the browser**: The shark is a DOM overlay on the main webview. Thanks to
  soksak's layer inversion (DOM is always composited above native browser webviews), the
  shark swims naturally over browser panels too.

## Performance (decorative — zero impact on the app)

- **Isolated compositing layer**: `position:fixed` + `contain:strict` + wrapper
  `will-change:transform` means shark animation does not trigger repaints on the app body
  (terminal).
- **30fps cap** — half the cost of a decorative layer.
- **rAF fully stopped when occluded or toggled off** — zero cost when not visible
  (`visibilitychange` + WebKit occlusion throttle).
- **Zero layout reads per frame** — window size is cached, updated only on resize.
- **`mousemove` attached only in reactive mode**.

## License

MIT
