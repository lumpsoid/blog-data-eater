---
title: "Using xinitrc to Start NixOS"
date: 2023-07-02
---

# Configuration.nix
To begin, you'll need to add the following lines to your configuration.nix file:
```nix
services.xserver.displayManager.startx.enable = true;
```

This enables the startx display manager service in NixOS, allowing you to use xinitrc for customization.

# Creating .xinitrc
Once you've made this change, you'll need to create a .xinitrc file in your home directory. The contents of this file should resemble the following:
```bash
#!/bin/sh
userresources=$HOME/.Xresources
usermodmap=$HOME/.Xmodmap
sysresources=/etc/X11/xinit/.Xresources
sysmodmap=/etc/X11/xinit/.Xmodmap
# merge in defaults and keymaps
if [ -f $sysresources ]; then
    xrdb -merge $sysresources
fi
if [ -f $sysmodmap ]; then
    xmodmap $sysmodmap
fi
if [ -f "$userresources" ]; then
    xrdb -merge "$userresources"
fi
if [ -f "$usermodmap" ]; then
    xmodmap "$usermodmap"
fi
# start some nice programs
if [ -d /etc/X11/xinit/xinitrc.d ] ; then
 for f in /etc/X11/xinit/xinitrc.d/?*.sh ; do
  [ -x "$f" ] && . "$f"
 done
 unset f
fi

lxqt-policykit-agent &
dunst &  # notification daemon
dwmblocks &  # bar for dwm
exec dwm  # window manager
```

These lines configure various settings and programs to be executed when X is started.

## Using nix home-manager
If you're using Home Manager to manage your user configuration files, you'll need to add the following lines to your home.nix or any other file you use with Home Manager:
```nix
".xinitrc".text = ''
    #!/bin/sh
    userresources=$HOME/.Xresources
    usermodmap=$HOME/.Xmodmap
    sysresources=/etc/X11/xinit/.Xresources
    sysmodmap=/etc/X11/xinit/.Xmodmap
    # merge in defaults and keymaps
    if [ -f $sysresources ]; then
        xrdb -merge $sysresources
    fi
    if [ -f $sysmodmap ]; then
        xmodmap $sysmodmap
    fi
    if [ -f "$userresources" ]; then
        xrdb -merge "$userresources"
    fi
    if [ -f "$usermodmap" ]; then
        xmodmap "$usermodmap"
    fi
    # start some nice programs
    if [ -d /etc/X11/xinit/xinitrc.d ] ; then
     for f in /etc/X11/xinit/xinitrc.d/?*.sh ; do
      [ -x "$f" ] && . "$f"
     done
     unset f
    fi
    autokey-gtk &
    lxqt-policykit-agent &
    dunst &
    dwmblocks &
    exec dwm
'';
```

# Rebuild NixOS
Make sure to rebuild your NixOS configuration after making these changes. You can do this by running the following command:
```
sudo nixos-rebuild switch
```

If you're using flakes to configure your system, the command would be slightly different:
```
sudo nixos-rebuild switch --flake <path/to/flake>#<your config name>
```

# Startx
After rebuilding, restart your system and log in as your user. Finally, execute the `startx` command, and if your graphics driver is properly configured, everything should work seamlessly.
