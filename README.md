# LiteCord
​
> A lightweight, unofficial Electron wrapper for Discord web with optional resource controls, a dedicated Crystal popup, tray notifications, and a guided screen-sharing picker.
​
**Project version:** 1.3.3
​
## GitHub repository description
​
**Lightweight Electron wrapper for Discord web with optional performance controls, a dedicated Crystal popup, tray notifications, and a guided screen-sharing picker.**
​
## Overview
​
LiteCord opens [Discord](https://discord.com/app) in an Electron desktop window. It adds a system-tray menu, optional UI and media optimizations, a dedicated in-app window for [Crystal](https://crystal.lat/), and an app-controlled screen/window picker for Crystal screen sharing.
​
LiteCord is an unofficial project. It is not affiliated with, endorsed by, or supported by Discord or Crystal. The Discord and Crystal interfaces are remote websites, so their behavior can change independently of LiteCord.
​
## Features
​
- **Discord web in a desktop window** with a single-instance lock and saved window size/position.
- **Crystal popup:** links to `crystal.lat` opened from Discord are routed into LiteCord's Crystal window instead of the default browser. Crystal uses a separate persistent browser session.
- **GPU controls:** automatic detection/fallback, force hardware acceleration, or use software rendering.
- **Optional resource controls:** an Extreme mode, animated-media blocking, background animation pausing, idle suspension, and manual memory cleanup.
- **Tray menu:** show/hide LiteCord, manage performance options, inspect process CPU/memory, and clear the web cache.
- **Optional unread badge:** uses Discord's unread count in the page title; Windows also receives a taskbar overlay badge.
- **Crystal screen-share picker:** when Crystal requests display capture, LiteCord presents a confirmation window for selecting a screen or application window.
​
## Requirements
​
- Node.js and npm.
- An internet connection for Discord and Crystal.
- A supported desktop environment for the desired Electron build target.
​
## Run locally
​
Open the project folder (the one containing `package.json`) in a terminal:
​
```bash
npm install
npm start
```
​
## Build installers
​
```bash
npm run build:win
npm run build:linux
npm run build:mac
```
​
Build artifacts are written to `dist/`. The configured targets are Windows NSIS and portable packages, Linux AppImage and DEB packages, and a macOS DMG. Build and test on the target operating system where possible; platform signing and permission requirements may apply.
​
For a folder-based test build:
​
```bash
npm run pack
```
​
## Tray menu and performance settings
​
Right-click the LiteCord tray icon to access these controls:
​
- **Hardware acceleration:** Automatic (recommended), Always on, or Off. Software rendering can substantially increase CPU use; use it only when the hardware renderer is unstable.
- **Extreme mode:** removes more visual effects and enables Chromium's low-end-device mode. Some pages may look less polished or scroll differently.
- **Block GIFs and videos:** reduces animated-media work but hides supported animated media from the Discord CDN.
- **Pause background animations:** pauses LiteCord's CSS animations when the main window loses focus; Chromium also applies its own background throttling.
- **Suspend while hidden:** unloads Discord after the selected idle period. Notifications stop while the page is suspended; active audio/calls are checked before suspension.
- **Release memory now / View CPU and memory:** diagnostic and cleanup actions in the tray menu.
- **Show notification count:** displays the unread count reported in Discord's window title. This depends on Discord continuing to use a recognizable title format.
​
Discord itself is a large web application. LiteCord cannot make CPU use zero while Discord is active, and channel switches may still cause work inside Discord's own interface. Hardware acceleration, animated media, server size, extensions/features on the page, and the computer's graphics drivers all affect performance.
​
## Crystal screen sharing
​
Start screen sharing from Crystal's own interface. When Crystal calls the browser display-capture API, LiteCord opens its screen/window selector. Select a source and confirm to begin; canceling denies that capture request.
​
The picker does not generate source thumbnails, which avoids extra preview capture work, as recommended by Electron's `desktopCapturer` API. [Electron documentation](https://www.electronjs.org/docs/latest/api/desktop-capturer) Crystal controls the requested resolution and frame rate.
​
### System audio limitation
​
The optional system-audio checkbox is shown only when Crystal requests audio and the current platform supports Electron's system loopback mode. On Windows, enabling it shares **all system output**, including LiteCord/Discord voices and other applications. Leave it unchecked to avoid retransmitting voices; game audio will then not be included either.
​
The current Electron screen-capture API exposes system loopback, not a per-application audio exclusion switch. [Electron session API](https://www.electronjs.org/docs/latest/api/session) LiteCord therefore does not claim to exclude Discord/LiteCord audio while still sharing game audio. Windows has a separate native API for process-tree loopback and exclusion, but integrating that audio into Crystal would require a native capture component and additional Windows-specific testing. [Microsoft Application Loopback sample](https://learn.microsoft.com/en-us/samples/microsoft/windows-classic-samples/applicationloopbackaudio-sample)
​
Screen capture also depends on OS permissions and Crystal's implementation. On macOS, grant screen-recording permission if requested. On Linux, the available source picker depends on the desktop session and PipeWire support.
​
More implementation notes: [Screen-sharing notes](SCREEN-SHARING-NOTES.md).
​
## Configuration
​
LiteCord creates `litecord-config.json` in its Electron user-data folder:
​
- Windows: `%APPDATA%/LiteCord`
- Linux: `~/.config/LiteCord`
- macOS: `~/Library/Application Support/LiteCord`
​
Supported configuration keys:
​
| Key | Default | Description |
|---|---:|---|
| `mode` | `balanced` | `balanced` or `extreme`. |
| `gpuMode` | `auto` | `auto`, `on`, or `off`. Changing this setting requires a restart. |
| `closeToTray` | `true` | Keep LiteCord running in the tray when the main window is closed. |
| `startMinimized` | `false` | Start hidden in the tray. |
| `splash` | `true` | Show the startup splash screen. |
| `splashExtraMs` | `350` | Extra splash delay, capped at 800 ms. |
| `blockTelemetry` | `true` | Blocks selected analytics endpoints used by the remote site. |
| `blockAnimatedMedia` | `false` | Blocks supported GIF/video files from Discord's media CDN. |
| `throttleUnfocused` | `true` | Pause CSS animations while LiteCord is unfocused. |
| `showNotificationCount` | `false` | Show the Discord title's unread count in the tray/taskbar badge. |
| `sleepAfterMin` | `0` | Minutes hidden before unloading Discord; `0` disables suspension. |
| `trimUnfocusedSec` | `90` | Seconds without focus before requesting renderer garbage collection; `0` disables it. |
​
## Security and privacy notes
​
- Discord and Crystal load as remote web content. LiteCord does not provide its own chat or authentication service.
- Browser windows use context isolation, sandboxing, and disabled Node integration.
- The Crystal popup has a separate persistent session from Discord.
- Screen sharing is limited to Crystal-origin requests with a user gesture, and the user must explicitly select and confirm a source.
- External HTTP(S) links outside the Crystal domain are opened in the default browser.
- Analytics blocking is a best-effort filter and may become outdated as the remote site changes.
​
## Known limitations
​
- LiteCord wraps the web apps; it cannot control their server-side behavior or guarantee low CPU use.
- Crystal must use the standard display-capture API for LiteCord's custom selector to appear.
- System audio is all-or-nothing in the current implementation; there is no per-app audio mix/exclusion.
- The notification badge relies on Discord's page-title format.
- Screen capture, audio availability, packaging, and permissions vary by operating system and should be tested on the target device.
​
## Troubleshooting
​
- **High CPU:** check the tray menu's GPU status. If it reports software rendering, try **Hardware acceleration → Automatic** and restart. For lower animation/media load, try Extreme mode or block GIFs/videos.
- **Crystal opens in a browser:** confirm the link uses `crystal.lat` or `www.crystal.lat`, then update to the latest LiteCord build.
- **No screen sources:** check OS screen-recording permissions and that Crystal initiated a screen-share request. If the Crystal site does not call the browser display-capture API, LiteCord cannot show its picker.
- **Build fails:** run `npm install` from the folder containing `package.json`, then use the build command for your operating system.
​
## Disclaimer
​
Use LiteCord at your own risk and follow the applicable terms and policies of the services you access. Always review security, privacy, and platform requirements before distributing a build.
​
