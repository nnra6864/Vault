---
created: 2026-01-02 12:09:25
tags:
  - note
  - tip
  - linux
  - config
---
Auto login is useful when already using disk encryption, which requires a password, or wanting to skip the login to be able to use remote access tools, such as [[Sunshine]].

## TTY
```
sudo bash -c "mkdir -p /etc/systemd/system/getty@tty1.service.d/ && cat > /etc/systemd/system/getty@tty1.service.d/autologin.conf << EOF
[Service]
ExecStart=
ExecStart=-/sbin/agetty -o '-p -f -- \\\\u' --noclear --autologin $USER %I \$TERM
EOF
" && sudo systemctl daemon-reload
```