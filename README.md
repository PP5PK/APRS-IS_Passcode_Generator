# 📡 APRS Passcode Generator

A web tool for generating **APRS-IS** (*Automatic Packet Reporting System – Internet Service*) passcodes from an amateur radio station callsign.

Built with a retro terminal aesthetic, no external runtime dependencies, and fully client-side — the calculation runs entirely in the browser, no data is ever sent to a server.

---

## 🌐 Live

> **[aprs.dvbr.net](https://aprs.dvbr.net)**

---

## ✨ Features

- **Auto-calculation with debounce** — passcode is generated automatically 2 seconds after typing stops (minimum 3 characters)
- **Instant calculation via Enter** — pressing `Enter` triggers the calculation immediately, bypassing the debounce delay
- **Auto-copy to clipboard** — as soon as the passcode is calculated, it is copied automatically
- **Manual copy button** — allows copying again at any time after the initial calculation
- **Real-time input sanitization** — only alphanumeric characters are accepted; input is forced to uppercase on the fly
- **Character counter** — with visual warning color as the 8-character limit approaches
- **Calculation status indicator** — animated "waiting" state (amber) and "done" confirmation (green)
- **Clear button** — appears dynamically while typing; resets all state with one click
- **Responsive layout** — adapted for both mobile and desktop screens
- **Bilingual UI (EN / PT)** — automatic language detection with a manual toggle; preference is saved across visits

---

## 🔐 Algorithm

The passcode is computed using the standard APRS-IS algorithm, derived from the uppercased station callsign:

```javascript
function calcPasscode(callsign) {
  var i = 0, code = 29666;
  while (i < callsign.length) {
    code = code ^ (callsign.charCodeAt(i) * 256);
    code = code ^ (callsign.charCodeAt(i + 1) || 0);
    i += 2;
  }
  return code & 32767;
}
```

The result is an integer between 0 and 32767, uniquely tied to the provided callsign.

---

## 🌍 Internationalization (i18n)

The page is available in **English** and **Portuguese (pt-BR)** within a single HTML file.

**Automatic detection** — on load, the script reads `navigator.language`. Any `pt-*` variant (pt-BR, pt-PT, etc.) renders the page in Portuguese; any other language falls back to English.

**Manual toggle** — a `🌐 PT | EN` button fixed in the top-right corner lets the user switch at any time. The active language is highlighted in green.

**Persistent preference** — the manual choice is saved to `localStorage` under the key `aprs-lang` and takes precedence over browser detection on subsequent visits.

**Translated elements** — every visible string switches with the language, including: page title, meta description, `<html lang>`, header label, card title and subtitle, field label, input placeholder, button labels, debounce status messages, and footer attribution.

---

## 🎨 Design

- **Theme:** dark retro terminal with scanline overlay and green/amber glow
- **Color palette:** background `#080c0e`, accent `#00e5a0` (green), highlight `#ffb347` (amber)
- **Typography:** [Orbitron](https://fonts.google.com/specimen/Orbitron) (headings and values) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) (interface text)
- **Accessibility:** high contrast, no reliance on red/green distinction for critical information

---

## 🏗️ Project Structure

```
.
├── index.html    # Complete application — HTML, CSS and JavaScript in a single file
└── favicon.svg   # Retro terminal antenna icon for browser tab
```

No frameworks, no build step, no runtime dependencies. Just open `index.html` in any modern browser.

---

## 🚀 Deployment

The project can be hosted on any static hosting service (Cloudflare Pages, GitHub Pages, Nginx, Apache, etc.).

Example Nginx configuration:

```nginx
server {
    listen 80;
    server_name aprs.dvbr.net;
    root /var/www/aprs;
    index index.html;
}
```

---

## 🔧 Browser Compatibility

| Feature | Support |
|---|---|
| Clipboard API (`navigator.clipboard`) | Modern browsers (HTTPS required) |
| `execCommand` fallback | Legacy browsers / HTTP |
| Language detection (`navigator.language`) | All modern browsers |
| `localStorage` preference | All modern browsers |
| Responsive layout | Mobile and desktop |
| JavaScript required | Yes (client-side calculation) |

---

## 📻 About APRS-IS

**APRS-IS** is the internet backbone that connects APRS gateways around the world, enabling tracking and data exchange between amateur radio stations. To connect a client application (such as Xastir, YAAC, Direwolf, etc.) to an APRS-IS server, a passcode derived from the station callsign is required.

Reference: [www.aprs-is.net](http://www.aprs-is.net)

---

## 👤 Author

**Daniel K. — PP5KX**

- Website: [pp5kx.net](https://pp5kx.net)
- WhatsApp: [Contact](https://api.whatsapp.com/send?phone=5541991912000)
- Telegram: [Contact](https://t.me/PP5KX)

---

## 📄 License

This project is freely available for use by the amateur radio community.
Feel free to adapt and redistribute it, keeping attribution to the original author.
