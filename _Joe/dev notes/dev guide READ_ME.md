# Route Plotter - Developer Guide

## 🚀 Quick Start

### Start Development Server

```bash
npm run dev
```

Opens at <http://localhost:3000> with auto-reload on file changes.

### Stop the Server

Press `Ctrl + C` in the terminal.

### If Port 3000 is Busy

```bash
# Find what's using port 3000
lsof -i :3000

# Kill it (replace PID with the number shown)
kill -9 PID
```

### Build for Production

```bash
npm run build          # Creates dist/ folder
npm run build:deploy   # Creates docs/ for GitHub Pages
```

### Version Numbering

The app displays version like `v3.1.168` in the header.

- **Format**: `major.minor.build`
- **major.minor**: Set manually in `package.json`
- **build**: Auto-increments in `version.json` on each JS rebuild

**When does version increment?**

| Change Type | Version Increments? |
|-------------|---------------------|
| Edit JS files (`src/`) | ✅ Yes (esbuild rebuilds) |
| Edit CSS/HTML | ❌ No (only copied, no rebuild) |
| Run `npm run build` | ✅ Yes |
| Restart `npm run dev` | ✅ Yes |

**Force a version bump** (useful after CSS-only changes):

```bash
# Touch a JS file to trigger rebuild
touch src/main.js
```

Or simply restart the dev server.

### Browser Showing Old Version?

If the browser still shows an old version after changes:

1. **Kill any zombie server processes**

   ```bash
   # Find the PID
   lsof -i :3000
   
   # Kill it
   kill -9 <PID>
   ```

2. **Clear dist and rebuild**

   ```bash
   rm -rf dist && npm run dev
   ```

3. **Hard refresh browser** — `Cmd+Shift+R` (Mac) or `Ctrl+Shift+R` (Windows/Linux)

4. **Clear browser cache** — DevTools → Application → Storage → Clear site data

---

## 📁 Project Structure

```text
src/
├── main.js              # 🎯 App entry point & orchestrator
├── config/              # Constants & settings
├── controllers/
│   └── UIController.js  # 🎛️ All UI interactions
├── models/
│   └── Waypoint.js      # 📍 Waypoint data model
├── services/
│   ├── AnimationEngine.js    # ⏱️ Playback & timing
│   ├── RenderingService.js   # 🎨 Canvas drawing
│   ├── PathCalculator.js     # 📐 Path math (curves, etc.)
│   └── MotionVisibilityService.js  # 👁️ Waypoint show/hide
└── core/
    └── EventBus.js      # 📡 Event communication

index.html               # Page structure
styles/main.css          # All styling
```

---

## 🏗️ Architecture Overview

### Event-Driven Pattern
Components communicate via **EventBus** (pub/sub), not direct calls:

```text
User clicks slider → UIController emits event → main.js handles → Services update
```

### Key Files to Know

| File | What It Does |
|------|--------------|
| `main.js` | Connects everything, handles events, manages state |
| `UIController.js` | Sidebar controls, waypoint list, tab switching |
| `Waypoint.js` | Data model for each point on the map |
| `RenderingService.js` | Draws everything on canvas |
| `AnimationEngine.js` | Controls playback timing |

### Data Flow

1. **User action** → UIController emits event
2. **main.js** catches event, updates data
3. **main.js** calls `queueRender()`
4. **RenderingService** draws the frame

---

## 💡 Tips for AI-Assisted Development

### When Asking for Changes

- Be specific: *"Change the waypoint dot color picker to also update the label color"*
- Reference files: *"In UIController.js, add a new slider for..."*
- Describe the UX: *"When user clicks X, Y should happen"*

### Before Making Changes

- **Build first**: `npm run build` - catches errors early
- **Test in browser**: Check console for errors (F12 → Console)
- **Save often**: Git commit working states

### Common Patterns in This Codebase

```javascript
// Emitting events (in UIController)
this.eventBus.emit('waypoint:color-changed', { waypoint, color });

// Handling events (in main.js)
this.eventBus.on('waypoint:color-changed', ({ waypoint, color }) => {
  waypoint.color = color;
  this.queueRender();
});

// Triggering a redraw
this.queueRender();  // Schedules next animation frame
```

### Debugging Tips

- Add `console.log('🔍', variableName)` to trace values
- Check browser DevTools → Network tab for failed loads
- Look for red errors in terminal or browser console

---

## 🔧 Common Tasks

### Add a New Waypoint Property

1. Add to `Waypoint.js` constructor & serialize with `toJSON()`
2. Add UI control in `index.html`
3. Add event listener in `UIController.js`
4. Handle event in `main.js`

### Change Styling

- Edit `styles/main.css`
- Changes auto-reload in dev mode

### Modify Canvas Drawing

- Edit `RenderingService.js`
- Look for `render*()` methods

---

## ⚠️ Gotchas

- **Don't edit `dist/`** - it's auto-generated
- **Imports at top** - never import mid-file
- **Canvas coordinates** - use `canvasToImage()` / `imageToCanvas()` for transforms
- **Autosave** - localStorage saves state; clear with browser DevTools if stuck

---

Happy coding! 🎉
