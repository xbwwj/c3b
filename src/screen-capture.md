# Screen Capture

## TL;DR

TODO: a diagram of architecture, and apis on platforms

Blink -> IPC -> Browser, HandleRequest permission

These platform APIs are used:

| Platform | API                                                 | Note                                |
| -------- | --------------------------------------------------- | ----------------------------------- |
| Linux    | [XDP Screencast][xdp] > [X-shm][xshm]               | PipeWire used internally by XDP     |
| Windows  | [WGC][wgc] > [DXGI Duplication][dxgid] > [GDI][gdi] |                                     |
| macOS    | [ScreenCaptureKit][sck] > [CoreDisplayStream][cds]  |                                     |
| Android  | [MediaProjection][amp]                              |                                     |
| Harmony  |                                                     | In OpenHarmony [webview fork][ohww] |
| iOS      |                                                     | Only implemented in WebKit          |

[wgc]: https://learn.microsoft.com/en-us/uwp/api/windows.graphics.capture?view=winrt-28000
[dxgid]: https://learn.microsoft.com/en-us/windows/win32/direct3ddxgi/desktop-dup-api
[gdi]: https://learn.microsoft.com/en-us/windows/win32/gdi/windows-gdi
[sck]: https://developer.apple.com/documentation/screencapturekit/
[cds]: https://developer.apple.com/documentation/coregraphics/cgdisplaystream
[xdp]: https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.ScreenCast.html
[xshm]: https://xorg.freedesktop.org/archive/X11R7.6/doc/man/man3/XShm.3.xhtml
[amp]: https://developer.android.com/reference/kotlin/android/media/projection/package-summary
[ohww]: https://gitcode.com/openharmony/web_webview

## Implementation details

### Chromium impl

- source:
  - `content/browser/media/capture/desktop_capture_device.cc`

All code run in Browser process (`desktopCaptureThread`) regardless of desktop or android.

## Platform discussion

### Linux screencast APIs

Wayland recently (2026) landed [`ext-image-capture-source-v1`][wics] and [`ext-image-copy-capture-v1`][wicc], which aim to provide native screencast/screenshot support across desktop environments. It evolved from vendor protocol `wlr-screencopy-unstable-v1` and `wlr-export-dmabuf-unstable-v1`.

[wics]: https://wayland.app/protocols/ext-image-capture-source-v1
[wicc]: https://wayland.app/protocols/ext-image-copy-capture-v1

These API should also supported in servo, becauses XDP is usually unavailable in minimalistic desktop environments, especially those used in **embedded scenarios** (weston, qt, wlroots and other tiling compositors).

XDP.Screencast should still be supported to run in flatpak sandboxed environments (e.g. SteamOS).

