---
created: 2026-09-09 07:28
tags:
  - note
  - guide
  - tip
  - pipewire
  - pulseaudio
  - audio
  - config
aliases:
---
[[Automatic Gain Control]] is arguably one of the most annoying things ever created, and for some reason, every single app an game seems to implement it.
If you just keep finding your input volume being different than what you set it to, that's it.
This guide assumes you are running [[PipeWire]] with [[Pulse Audio]] support.

## Fix

Simply run the following command:

```sh
mkdir -p ~/.config/pipewire/pipewire-pulse.conf.d && echo 'pulse.rules = [
    {
        matches = [
            { application.process.binary = "!~^(pavucontrol|pactl|hyprpwcenter)$" }
        ]
        actions = {
            quirks = [ block-source-volume ]
        }
    }
]' | tee ~/.config/pipewire/pipewire-pulse.conf.d/99-lock-mic-volume.conf > /dev/null
```

> [!NOTE]
> Add all the apps you want to allow to change the volume to the list.
> To get binary names, run the following command:
> ```sh
> pactl list clients | grep binary
> ```

Make sure to restart the services for changes to apply:
```sh
systemctl --user restart pipewire pipewire-pulse
```

Thanks [[Wim Tayams]] for pointing me in the right direction.
> [Wim Taymans](https://matrix.to/#/@wtay:matrix.org)
> [nnra](https://matrix.to/#/@nnra:matrix.org): block source volume with a pulseaudio quirk can help: https://docs.pipewire.org/page_module_protocol_pulse.html

https://matrix.to/#/!kySKEYzDwVhedDCSoX:matrix.org/$KVqVUSczfV3Q6tKVOFcYd9BrWt0S1sn9IcoUUOyDJTA?via=matrix.org&via=tchncs.de&via=mozilla.org