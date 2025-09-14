# fnac

An intentionally small static demo: an interactive "reset lever" designed to be loaded on various devices and used as part of a Five Nights at Freddy's (FNAF) style role-play. In that context each lever can represent an in-game system (power, lights, doors, vents, cameras, etc.).

## Files

- `reset-lever.html` — The interactive lever UI. Load this file on a device to use it as a control in your role-play.
- `README.md` — This document.

## Purpose & role-play usage

This project provides a simple, visual lever control that can be used by players or game masters during FNAF-inspired role-play sessions. Typical uses:

- Map each physical or virtual lever to an in-game system: power, lights, doors, cameras, HVAC, or emergency reset.
- Use multiple devices (phones/tablets/laptops) on the same local network; each device hosts one or more lever instances representing different controls.
- Game masters can monitor or trigger game events when players toggle levers, or use the lever as a prop for puzzles and timed sequences.

Design notes:

- The UI is intentionally simple so it loads quickly on low-powered devices and in mobile browsers.
- The lever is visual only — integrating it with an authoritative game state requires a small glue layer (see "Integration options").

## Device compatibility & loading

- Desktop browsers (Chrome, Firefox, Safari) — fully supported.
- Mobile browsers — supported on recent iOS and Android versions. Use the native browser for best performance.
- Kiosk or embedded devices — the file can be loaded offline if copied onto the device.

Loading options:

- Open the file directly in the browser (double-click or drag-and-drop).
- Host with a tiny local server (useful for camera/permission APIs or if you serve multiple devices):

```bash
# Python 3 (from repo root)
python3 -m http.server 8000
# Then open http://<host-ip>:8000/reset-lever.html on other devices
```

Tip: when using multiple devices on the same LAN, replace <host-ip> with the machine IP that runs the server (e.g., 192.168.1.10).

## Integration options

The HTML lever is standalone. To bind lever actions to an authoritative game state you can:

1. WebSocket server — run a small Node/Python server that accepts lever events and broadcasts state to all clients.
2. HTTP polling — have the lever POST state changes to a REST endpoint that the game master polls.
3. Manual — players report lever changes verbally or via a shared document; the GM updates the game state.

Example (minimal WebSocket flow):

- Each `reset-lever.html` instance connects to ws://game-server:3000.
- On toggle, client sends { "leverId": "power", "state": "on" }.
- Server validates, updates authoritative state, and broadcasts the new state back to all clients.

If you want, I can add a tiny example WebSocket server and update `reset-lever.html` to emit events.

## Customization

- Labels: change the text (e.g., from "Reset" to "Power") inside `reset-lever.html` to match your mapping.
- Colors / sounds: replace or add assets (images, audio) to fit your table-top role-play theme.
- Multiple levers: duplicate the lever markup inside the HTML and give each one a unique id/label.

Quick mapping contract (suggested):

- Input: lever toggles (id, on|off, timestamp)
- Output: UI update (visual state), optional network event to game server
- Error modes: offline (no server) — UI still works locally; network failure — queue or retry events on reconnect

## Accessibility & safety

- Provide clear labels for each lever so players with screen readers can understand the control.
- Avoid flashing or fast-moving visuals that may trigger photosensitive reactions.
- Content note: FNAF role-play may include jump scares or horror themes; warn participants accordingly.
