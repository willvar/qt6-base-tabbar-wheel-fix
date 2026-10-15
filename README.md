# Qt 6.12 QTabBar wheel fix for Arch Linux

## Status: prefer the official Arch package

As of 2026-10-15, Extra provides **`qt6-base 6.12.0-3`** with an official fix:
[Arch packaging commit](https://gitlab.archlinux.org/archlinux/packaging/packages/qt6-base/-/commit/070299bbd137a648d9da9d430eefb7ac47a55f1c).
It includes [Qt commit `6b7ee05`](https://code.qt.io/cgit/qt/qtbase.git/commit/?id=6b7ee05def510c333fbb8944ae669b72a48367f3),
now merged upstream.

**Arch users should use the official fixed package through a normal full system
upgrade, rather than build this older `6.12.0-2` recipe.** The unchanged package
name and lower version allow pacman to replace this local build normally. Do not
hold it back or downgrade to this recipe for the original tab-switching issue.

## What the local patch does and does not do

This repository preserves an earlier local workaround: one additional condition
in Qt's shared `QTabBar::wheelEvent()`. On the investigated KDE Wayland system,
physical wheel events use the existing tab-switching branch, while synthesized
events from a `PixelScroll`-capable device retain the pixel-scrolling branch.

This is a **QTabBar-only workaround**, not a correction to the Wayland device
capability reporting. It does not address the other scrolling problems described
in the upstream commit. The official fix withdraws the incorrect capability
declaration at the Wayland input layer; it does not remove `pixelDelta()` delivery
or disable all touchpad scrolling.

The local patch uses [`QWheelEvent::source()`](https://doc.qt.io/qt-6/qwheelevent-obsolete.html#source),
which is deprecated since Qt 6.0. Qt documents that some platforms always return
`MouseEventNotSynthesized`, so this is not a portable hardware-type check. The
patch is not restricted to Wayland and has not been comprehensively tested on
other platforms or physical touchpads. These are limitations, not a claim of a
confirmed regression on another platform. The local workaround has not been
established as generally preferable to the official fix.

The two files named `qt6-base-cflags.patch` and `qt6-base-nostrip.patch` are
the existing Arch package patches. `qt6-base-tabbar-wheel-mouse.patch` is the
functional fix in this repository.

## Historical build instructions (`6.12.0-2` only)

<details>
<summary>For reference; not recommended over the fixed official package</summary>

```bash
git clone https://github.com/willvar/qt6-base-tabbar-wheel-fix.git
cd qt6-base-tabbar-wheel-fix
makepkg -si
```

No Dolphin, Konsole, or Kate rebuild is needed. Close and reopen affected
applications after installing Qt.

</details>

Before the official fix was available, an application could also be started
through XWayland as a temporary workaround, for example:

```bash
QT_QPA_PLATFORM=xcb dolphin
```

Related upstream discussion: <https://codereview.qt-project.org/c/qt/qtbase/+/778943>
