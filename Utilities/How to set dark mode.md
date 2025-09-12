## GTK

### Install `xdg-desktop-portal`

```sh
paru -S xdg-desktop-portal-hyprland xdg-desktop-portal-gtk
```

### Configuration

```toml
# ~/.config/xdg-desktop-portal/hyprland-portals.conf
[preferred]
default=hyprland;gtk
```

### Usage

```sh
# GTK 3
$ gsettings set org.gnome.desktop.interface gtk-theme "Adwaita-dark"

# GTK 4
$ gsettings set org.gnome.desktop.interface color-scheme "prefer-dark"
```

## Qt

### Install

```sh
paru -S qt5ct qt6ct
```

### Configuration

```hyprlang
env = QT_QPA_PLATFORMTHEME,qt6ct
```

### Usage

Use `qt5ct` and `qt6ct` to configure Qt5 and Qt6 theme.

> [!warning]
>
> Haven't looked into how to setup a theme and found a suitable theme.

## References

* [How to set dark mode?](https://github.com/hyprwm/Hyprland/discussions/5867)
* Google Gemini
