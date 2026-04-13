# Dot Crowd Navigator

Graph-based crowd-flow simulation tool. Draw a network of nodes and weighted edges over a background image, then run a swarm of dots through the network to visualise pedestrian or crowd flow patterns. Forked from Route Plotter; see `_Joe/Dot Crowd Navigator App Overview.md` for full context.

> **Note:** This README still contains legacy Route Plotter documentation below. A full rewrite is tracked in the backlog (Phase 1F).

## Features

**Waypoints** — Click to add major waypoints, Cmd/Ctrl+Click for minor control points. Drag to reposition. Each waypoint can have text labels, custom markers, beacon effects, wait times, and per-segment speed control.

**Path** — Catmull-Rom spline interpolation with adjustable tension. Line, squiggle, or randomised shapes. Solid, dashed, or dotted styles. Configurable thickness and colour from the Okabe-Ito colour-blind safe palette.

**Animation** — Constant-speed or constant-time modes. Corner slowing for natural motion. Multiple visibility modes for path, markers, and background — including spotlight, angle-of-view, and comet trail effects.

**Camera** — Per-waypoint zoom with continuous interpolation between keyframes.

**Export** — Video (WebM) or standalone HTML at custom resolution, aspect ratio, and frame rate. Export with or without the background image.

**Accessibility** — WCAG 2.2 AAA target. Full keyboard navigation, ARIA labels, screen reader announcements, focus trapping in modals, and Okabe-Ito colours throughout.

**Persistence** — Auto-saves to localStorage. Save/load projects as ZIP files.

## Quick start

```bash
git clone https://github.com/djDAOjones/router-plotter-02.git
cd router-plotter-02
npm install
npm run dev
```

Opens at [http://localhost:3000](http://localhost:3000).

```bash
npm run build   # Production build → docs/
npm test        # Run tests (Vitest)
```

## Project structure

```plaintext
├── index.html
├── build.js
├── package.json
├── src/
│   ├── main.js                  # RoutePlotter application class
│   ├── config/
│   │   ├── constants.js         # All tuneable values
│   │   ├── keybindings.js       # Mouse + keyboard bindings (customisable)
│   │   ├── helpContent.js       # Welcome modal content
│   │   └── tooltips.js          # Tooltip definitions
│   ├── components/              # SwatchPicker, Dropdown, Tooltip
│   ├── controllers/             # UIController, SectionController
│   ├── core/EventBus.js         # Pub-sub event system
│   ├── handlers/                # InteractionHandler (mouse + keyboard)
│   ├── models/                  # Waypoint, AnimationState, ImageAsset
│   └── services/                # AnimationEngine, PathCalculator,
│                                  RenderingService, CameraService,
│                                  BeaconRenderer, TextLabelService,
│                                  MotionVisibilityService, VideoExporter,
│                                  HTMLExportService, CoordinateTransform,
│                                  ImageAssetService, StorageService,
│                                  UndoService
├── styles/
│   ├── tokens.css               # Design tokens (UoN palette)
│   ├── main.css                 # Core layout and components
│   ├── swatch-picker.css        # Colour picker
│   ├── dropdown.css             # Dropdown menus
│   └── tooltip.css              # Tooltips
└── docs/                        # GitHub Pages build output
```

## Tech

Pure JavaScript — no frameworks. Canvas-based rendering, esbuild bundler, CSS custom properties for theming. Zero runtime dependencies.

---

## Configuration

All tuneable values live in `src/config/constants.js`. The tables below document each group.

### `ANIMATION`

| Constant | Default | Description |
|---|---|---|
| `DEFAULT_DURATION` | `10000` | Default animation duration (ms) |
| `DEFAULT_SPEED` | `400` | Default speed (px/s) |
| `DEFAULT_WAIT_TIME` | `1500` | Default waypoint pause (ms) |
| `TARGET_FPS` | `60` | Render target frame rate |
| `MAX_DELTA_TIME` | `100` | Cap on frame time jump (ms) |
| `TIMELINE_RESOLUTION` | `1000` | Timeline slider steps |

### `VIDEO_EXPORT`

| Constant | Default | Description |
|---|---|---|
| `DEFAULT_FRAME_RATE` | `25` | Export frame rate (fps) |
| `DEFAULT_BITRATE` | `20000000` | Video bitrate (20 Mbps) |
| `START_BUFFER_MS` | `2000` | Static frame at start of export (ms) |

### `RENDERING`

| Constant | Default | Description |
|---|---|---|
| `DEFAULT_PATH_COLOR` | `#D55E00` | Default path colour (Okabe-Ito Vermillion) |
| `DEFAULT_PATH_THICKNESS` | `3` | Line thickness (px, legacy) |
| `DEFAULT_DOT_SIZE` | `8` | Major waypoint radius (px, legacy) |
| `MINOR_DOT_SIZE` | `4` | Minor waypoint radius (px, legacy) |
| `MINOR_DOT_COLOR` | `#000000` | Minor waypoint colour |
| `MINOR_DOT_OPACITY` | `0.5` | Minor waypoint opacity |
| `PATH_HEAD_SIZE` | `8` | Path head marker radius (px, legacy) |
| `REFERENCE_DIAGONAL` | `1414` | Reference diagonal for relative sizing |
| `BEACON_PULSE_DURATION` | `2000` | Pulse cycle length (ms) |
| `BEACON_RIPPLE_DURATION` | `1500` | Ripple lifetime (ms) |
| `BEACON_RIPPLE_INTERVAL` | `500` | Time between ripples (ms) |
| `CONTROLS_HEIGHT` | `80` | Bottom controls panel height (px) |

### `PATH`

| Constant | Default | Description |
|---|---|---|
| `POINTS_PER_SEGMENT` | `100` | Catmull-Rom interpolation density |
| `DEFAULT_TENSION` | `0.1` | Curve tightness (lower = tighter) |
| `TARGET_SPACING` | `2` | Pixels between reparameterised points |
| `MIN_CORNER_SPEED` | `0.2` | Minimum speed at corners (20%) |
| `MAX_CURVATURE` | `0.1` | Curvature threshold for max slowing |

### `MOTION`

| Constant | Default | Description |
|---|---|---|
| `PATH_TRAIL_DEFAULT` | `0.20` | Trail length as fraction of path duration |
| `SPOTLIGHT_SIZE_DEFAULT` | `10` | Spotlight radius (% of canvas) |
| `SPOTLIGHT_FEATHER_DEFAULT` | `0` | Spotlight feather (% of spotlight) |
| `AOV_ANGLE_DEFAULT` | `60` | Angle-of-view cone angle (degrees) |
| `AOV_DISTANCE_DEFAULT` | `25` | AoV distance (% of canvas diagonal) |
| `AOV_DROPOFF_DEFAULT` | `50` | AoV gradient fade (%) |
| `TIMELINE_START_HANDLE_MS` | `2000` | Pre-animation static buffer (ms) |
| `TIMELINE_END_HANDLE_MS` | `3000` | Post-animation buffer (ms) |

### `INTERACTION`

| Constant | Default | Description |
|---|---|---|
| `WAYPOINT_HIT_RADIUS` | `15` | Click detection radius (px) |
| `DRAG_THRESHOLD` | `3` | Minimum drag distance (px) |
| `DOUBLE_CLICK_TIME` | `300` | Double-click window (ms) |

### `TEXT_LABEL`

| Constant | Default | Description |
|---|---|---|
| `SIZE_PX_MIN` / `MAX` | `16` / `48` | Font size range (px) |
| `WIDTH_DEFAULT` | `15` | Text area width (% of canvas) |
| `OFFSET_DEFAULT_Y` | `-5` | Default vertical offset (% above marker) |
| `BG_OPACITY_DEFAULT` | `0.85` | Label background opacity |
| `FADE_DURATION` | `500` | Fade in/out duration (ms) |
| `AUTO_POSITION_DIRECTIONS` | `8` | Directions tested for auto-position |

### Visibility modes

Defined as enums in `constants.js`:

- **Path** (`PATH_VISIBILITY`): `always-show`, `show-on-progression`, `hide-on-progression`, `instantaneous` (comet), `always-hide`
- **Waypoints** (`WAYPOINT_VISIBILITY`): `always-show`, `hide-before`, `hide-after`, `hide-before-and-after`, `always-hide`
- **Background** (`BACKGROUND_VISIBILITY`): `always-show`, `spotlight`, `spotlight-reveal`, `angle-of-view`, `angle-of-view-reveal`, `always-hide`
- **Text** (`TEXT_VISIBILITY`): `off`, `on`, `fade-up`, `fade-up-down`

---

## Custom keybindings

All mouse and keyboard shortcuts are defined in `src/config/keybindings.js` and can be customised at runtime via localStorage.

### How it works

Bindings are loaded from `DEFAULT_BINDINGS`, then merged with any user overrides stored under the `routePlotter_customKeybindings` localStorage key. Each binding has:

```javascript
{
  key: 'click',              // Key or mouse action
  modifiers: ['alt', 'meta'], // Required modifiers: meta, alt, shift
  action: 'waypoint:force-add-minor',  // EventBus event to emit
  description: 'Force add minor (bypass selection)',
  category: 'waypoint'       // Groups: waypoint, navigation, playback, general
}
```

`meta` maps to **Cmd** on macOS and **Ctrl** on Windows/Linux.

### Programmatic API

```javascript
import { getKeybindings, saveCustomBindings, resetToDefaults } from './config/keybindings.js';

// Read current bindings
const bindings = getKeybindings();

// Override a single keyboard binding
const custom = { keyboard: { playPause: { key: 'p' } } };
saveCustomBindings(custom);

// Reset everything
resetToDefaults();
```

### Default binding categories

The full set of defaults is defined in `keybindings.js` under `DEFAULT_BINDINGS.mouse` and `DEFAULT_BINDINGS.keyboard`, organised into four categories: **Waypoints**, **Navigation**, **Playback**, and **General**. The in-app help panel (press `?`) renders all bindings dynamically from this config.

---

## Glossary

Precise terminology for discussing features, bugs, and enhancements.

### Core concepts

- **Route** — The complete journey from first waypoint to last.
- **Path** — The interpolated Catmull-Rom spline connecting waypoints.
- **Path points** — The array of calculated coordinates defining the path (typically hundreds of points).
- **Canvas** — The HTML5 canvas element where all rendering occurs.
- **Background image** — The map or image displayed behind the route.

### Waypoints and markers

- **Waypoint** — A user-defined point along the route. Either *major* or *minor*.
- **Major waypoint** — Full-featured: labels, pause times, beacon effects, larger marker (8px default).
- **Minor waypoint** — Path shaping only: smaller marker (4px), no pause or label support.
- **Marker** — The visual representation of a waypoint (dot, square, flag, custom image, or none).
- **Selected waypoint** — Currently being edited. Highlighted with a yellow glow.

### Path and curves

- **Tension** — Controls curve tightness (0.0 = straight lines, 1.0 = maximum smoothness). Default: 0.1.
- **Path shape** — Line (smooth Catmull-Rom), squiggle (sine wave modulation), or randomised (jittered).
- **Curvature** — How sharply the path bends at any point. Drives corner slowing.
- **Corner slowing** — Automatic speed reduction at sharp turns. Controlled by `MIN_CORNER_SPEED`.
- **Reparameterisation** — Redistributing path points at even spacing for consistent animation speed.
- **Segment speed** — Per-segment speed multiplier (0.1x–10x) for variable-speed animation.

### Animation and timing

- **Progress** — Position through the animation, 0.0 (start) to 1.0 (end).
- **Duration** — Total animation length in milliseconds, calculated from path length ÷ speed.
- **Speed** — Animation velocity in px/s (default 400).
- **Playback speed** — Multiplier on animation speed (0.1–10.0). Controlled by J/K/L keys.
- **Timeline** — The scrubber/slider showing animation progress (0–1000 steps).
- **Playhead** — The moving indicator at the current path position (arrow, dot, or custom image).
- **Waypoint pause** — Timed pause, manual pause (wait for user), or none.

### Visual effects

- **Beacon** — Animated effect at major waypoints: pulse, ripple, glow, pop, or grow.
- **Path head** — The leading indicator (arrow, dot, custom image, or none).
- **Label** — Text at major waypoints with visibility modes: off, always on, fade up, fade up & down.
- **Tint** — Semi-transparent overlay on the background image (−100 black to +100 white).
- **Spotlight** — Circular reveal around the path head. Configurable size and feather.
- **Angle of view** — Cone-shaped reveal from the path head with adjustable angle, distance, and dropoff.
- **Trail** — In comet/instantaneous mode, the visible portion of the path behind the head.

### Coordinate systems

- **Image coordinates** (`imgX`, `imgY`) — Position in the original image. Used for waypoint storage.
- **Canvas coordinates** (`x`, `y`) — Screen position on the display canvas. Used for rendering and interaction.
- **Coordinate transform** — Conversion between the two systems, accounting for zoom, pan, and fit/fill mode.

### Architecture

- **EventBus** — Pub-sub system for decoupled communication between services.
- **Services** — Modular components: AnimationEngine, PathCalculator, RenderingService, CameraService, BeaconRenderer, TextLabelService, MotionVisibilityService, VideoExporter, HTMLExportService, CoordinateTransform, ImageAssetService, StorageService, UndoService.
- **RoutePlotter** — Main application class in `src/main.js` coordinating all services.

---

## License

MIT — see LICENSE file.

## Author

Joe Bell — University of Nottingham

## Links

- [Repository](https://github.com/djDAOjones/router-plotter-02)
- [Live demo](https://djdaojones.github.io/router-plotter-02/)
- [Issues](https://github.com/djDAOjones/router-plotter-02/issues)
