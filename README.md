# Stream Overlay Engine

A retrowave-styled Twitch stream overlay with animated neon borders, scanlines, and reactive reaction frames.

## Pages

### `index.html`
The original overlay page with an ambient CRT glitch background effect. Features the same retrowave frame and reactive iframes as `index2.html`, plus a generative glitch canvas animation in the background.

### `index2.html`
The clean overlay page without the glitch background. Features a fully animated retrowave frame with neon edges, scanlines, and a sunset grid.

## How to Use

Serve both files via any local HTTP server (required for WebSocket and iframe functionality):

```bash
python -m http.server 8080
# or
npx serve .
```

Then open in browser:

```
http://localhost:8080/index.html?channel=YOUR_CHANNEL&user=TARGET_USER
http://localhost:8080/index2.html?channel=YOUR_CHANNEL&user=TARGET_USER
```

### URL Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `channel` | Twitch channel to connect to | `technogumanitarij` |
| `user` | Twitch username to listen for commands | (same as `channel`) |

### In-Chat Commands

| Command | Source | Action |
|---------|--------|--------|
| `!boom` | Owner | Triggers `boom-frame` for 5000ms |
| `!dance` | Owner or Viewer | Triggers `dance-frame` for 5000ms |
| `!boom` | Viewer | Triggers `question-frame` with viewer username for 5000ms |

## Reaction Files

Located in `reactions/`:

- `boom.html` — Boom reaction iframe (referenced as `boom.html`)
- `dance.html` — Dance reaction iframe (referenced as `dance.html`)
- `dance_question.html` — Question reaction iframe (referenced as `question.html` in code)

## Architecture

- **WebSocket** connects to `wss://irc-ws.chat.twitch.tv:443` for real-time chat listener
- **Iframes** load reaction HTML files and receive `postMessage` commands
- **Retrowave frame** (index2.html) uses pure CSS animations — no JavaScript required for the visual frame
- **Z-index layering**: Retrowave frame (z-index: 0) → Iframes (z-index: 3)

## Customization

### Change neon colors
Edit the `color`, `background`, and `box-shadow` values in `.edge-*` and `.corner-*` classes in `index2.html`.

### Adjust animation speed
Modify `animation-duration` on `.edge` and `.corner`:
```css
.retrowave-frame .edge { animation: neonPulse 2s ease-in-out infinite alternate; }
```

### Change frame thickness
Edit `width`/`height` on `.edge-*`:
```css
.retrowave-frame .edge-top { height: 30px; }
```
