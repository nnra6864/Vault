---
created: 2026-08-18 08:02
tags:
  - note
  - git
  - tip
aliases:
---
If you have a repo with many submodules registered under `git.nnstdios.xyz`, you can easily update them all to `git.nnstdios.com` with 1 command.
If you were smart and used a relative git path when adding submodules, e.g.:
```sh
git submodule add ../../nnra6864-mirrors/some-module modules/some-module
```
Just run:
```sh
git submodule sync --recursive
```

If you didn't use relative git paths, you must first update all paths in the `.gitmodules` file, and then run the command above.