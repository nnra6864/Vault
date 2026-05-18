---
created: 2026-05-15 14:09
tags:
  - note
  - tip
  - guide
  - linux
  - container
  - config
aliases:
---

## Environment Variables and Paths

By default, [[Flatpak]] doesn't have permissions to access your config directories and environment variables for security reasons.
To make it work, you'll have to give it readonly permissions for specific dirs, and pass some system env vars.
Following should cover most, if not all of them:
```sh
flatpak override --user \
    --env=XCURSOR_THEME=$XCURSOR_THEME \
    --env=XCURSOR_SIZE=$XCURSOR_SIZE \
    --env=GDK_BACKEND=$GDK_BACKEND \
    --env=GDK_SCALE=$GDK_SCALE \
    --env=GTK_THEME=$GTK_THEME \
    --env=QT_QPA_PLATFORM=$QT_QPA_PLATFORM \
    --env=QT_QPA_PLATFORMTHEME=$QT_QPA_PLATFORMTHEME \
    --env=QT_QUICK_CONTROLS_STYLE=$QT_QUICK_CONTROLS_STYLE \
    --env=QT_AUTO_SCREEN_SCALE_FACTOR=$QT_AUTO_SCREEN_SCALE_FACTOR \
    --env=QT_SCALE_FACTOR=$QT_SCALE_FACTOR \
    --env=QT_WAYLAND_DISABLE_WINDOWDECORATION=$QT_WAYLAND_DISABLE_WINDOWDECORATION \
    --env=CLUTTER_BACKEND=$CLUTTER_BACKEND \
    --env=ELECTRON_OZONE_PLATFORM_HINT=$ELECTRON_OZONE_PLATFORM_HINT;

flatpak override --user \
    --filesystem=xdg-data/themes:ro \
    --filesystem=xdg-data/icons:ro \
    --filesystem=xdg-data/fonts:ro \
    --filesystem=xdg-config/fontconfig:ro \
    --filesystem=xdg-config/gtk-3.0:ro \
    --filesystem=xdg-config/gtk-4.0:ro \
    --filesystem=xdg-config/qt5ct:ro \
    --filesystem=xdg-config/qt6ct:ro \
    --filesystem=xdg-config/Kvantum:ro \
    --filesystem=xdg-config/kdeglobals:ro
```

> [!WARNING]
> Make sure your host actually sets these environment variables.

> [!WARNING]
> Passed environment variables are evaluated at the time of running the command, they won't be dynamically updated.
> I personally use a `rice` script that handles updating these for me by rerunning the command, but use the solution that works the best for you.