---
created: 2026-05-07 19:17
tags:
  - note
  - tip
  - dev/server
  - dev/hosting
  - ai
aliases:
---
This usually happens when you put [[Open WebUI]] behind a reverse proxy such as [[Angie]]/[[NGINX]].
By default, they use HTTP 1.0, which doesn't support [[Web Socket]]s.
To enable web sockets, you should add the following to your server's `location` block:

```
# WebSocket support
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```

On top of that, you can add the following to allow for words appearing instantly:

```
# Allows word by word instead of chunk updates
proxy_buffering off;
```