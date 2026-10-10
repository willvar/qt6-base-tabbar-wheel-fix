# Qt 6.12 QTabBar wheel fix for Arch Linux

This is an Arch `qt6-base` package recipe with one additional patch for the
Qt 6.12 Wayland tab-bar regression. It restores physical mouse-wheel tab
switching in applications using the standard Qt `QTabBar`, while preserving
pixel scrolling for synthesized touchpad events.

The two files named `qt6-base-cflags.patch` and `qt6-base-nostrip.patch` are
the existing Arch package patches. `qt6-base-tabbar-wheel-mouse.patch` is the
functional fix in this repository.

## Build and install

```bash
git clone https://github.com/willvar/qt6-base-tabbar-wheel-fix.git
cd qt6-base-tabbar-wheel-fix
makepkg -si
```

No Dolphin, Konsole, or Kate rebuild is needed. Close and reopen affected
applications after installing Qt.

As a temporary Wayland workaround, an application can also be started through
XWayland, for example:

```bash
QT_QPA_PLATFORM=xcb dolphin
```

The package follows Arch's `qt6-base` recipe at version `6.12.0-2`. Check the
upstream Qt status before using it with a newer package version.

Related upstream discussion: <https://codereview.qt-project.org/c/qt/qtbase/+/778943>
