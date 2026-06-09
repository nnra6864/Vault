---
created: 2025-12-27 08:21:23
tags:
  - note
  - tip
  - linux
  - game
---
This is usually caused by wine not handling loading [[DLL]]s properly.
It can easily be fixed by using the following launch option:
```
WINEDLLOVERRIDES="winhttp=n,b"
```

[Source](https://bsmg.wiki/linux-modding.html#dll-override)