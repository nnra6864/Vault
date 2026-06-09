---
created: 2026-05-13 12:58
tags:
  - note
  - linux
  - config
  - tip
  - guide
  - nvidia
aliases:
---
You can squeeze a bit of extra performance out of your [[Nvidia]] [[GPU]] by increasing its default power limit to the maximum value.
To get this value, run:

```sh
nvidia-smi -q -d POWER | grep "Power Limit" | grep -v "N/A"
```

If you `Default Power Limit`/`Requested Power Limit` is lower than `Max Power Limit`, like in my case:

```
nvidia-smi -q -d POWER | grep "Power Limit" | grep -v "N/A"
        Current Power Limit                            : 170.00 W
        Requested Power Limit                          : 170.00 W
        Default Power Limit                            : 170.00 W
        Min Power Limit                                : 100.00 W
        Max Power Limit                                : 212.00 W
```

Run the following:

```sh

echo "[Unit]
Description=Set NVIDIA TDP
After=network.target nvidia-persistenced.service
Wants=nvidia-persistenced.service

[Service]
Type=oneshot
ExecStartPre=/usr/bin/sleep 2
ExecStartPre=/usr/bin/nvidia-smi -pm 1
ExecStart=/usr/bin/nvidia-smi -pl INSERT_Max_Power_Limit_HERE
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target" \
| sudo tee /etc/systemd/system/nvidia-tdp.service > /dev/null && \
sudo systemctl enable --now nvidia-tdp
```

> [!WARNING]
> Make sure to replace `INSERT_Max_Power_Limit_HERE` with your actual max power limit.