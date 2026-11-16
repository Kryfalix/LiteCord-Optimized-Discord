# LiteCord
​
> An ultra-lightweight, performance-focused desktop wrapper for Discord web, with a dedicated Crystal popup and custom screen sharing.
​
## What is LiteCord?
​
LiteCord is an unofficial desktop app that brings the Discord web experience into a streamlined Electron window. It is designed to feel lightweight, optimized, and responsive while keeping the familiar Discord interface.
​
LiteCord focuses on reducing unnecessary work around the web app and adding practical desktop features. Performance depends on the computer, graphics drivers, and the activity and content loaded by Discord; the app does not promise zero CPU usage.
​
## What it does
​
- **Discord in a focused desktop app:** opens Discord web in a dedicated LiteCord window rather than a regular browser tab.
- **Performance-focused controls:** provides hardware-acceleration options, background animation pausing, optional animated-media blocking, an Extreme mode, and suspension while hidden.
- **Crystal integration:** opens Crystal in its own LiteCord popup. Links to Crystal opened from Discord are routed into that popup instead of the default browser.
- **Custom screen sharing for Crystal:** when Crystal requests screen capture, LiteCord presents its own screen/window picker and asks the user to confirm the source. The picker avoids preview thumbnails to keep the selection flow lightweight.
- **Optional notification count:** can show Discord's unread count on the tray icon and, on Windows, as a taskbar badge.
- **Desktop conveniences:** includes a system-tray menu, saved window size and position, and CPU/memory information.
​
## Custom screen sharing
​
The screen-sharing flow is designed to be clear and lightweight: choose a screen or window, confirm, and then Crystal receives the selected capture source. LiteCord does not add a separate video-encoding step; Crystal controls the requested capture quality and frame rate.
​
System audio is optional when requested by Crystal. On Windows, enabling it captures the full system audio mix—including LiteCord/Discord voices and other apps. Leave it off to avoid retransmitting voices; game audio will not be included either. Per-application audio exclusion is not currently supported.
​
## Project identity
​
LiteCord is a community project and is not affiliated with, endorsed by, or supported by Discord or Crystal. Discord and Crystal continue to run as remote web applications, so their interfaces and behavior may change independently of LiteCord.
​
