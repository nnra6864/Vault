---
created: 2026-05-11 06:10
tags:
  - note
  - tip
  - nvim
aliases:
---
[[Nvim]] Quickfix List is an insanely useful tool that allows you to easily view/execute operations on a selection of files.
A great example would be upgrading to the new [[API]] by replacing all instances of a word `CFramebuffer` with `SP<Render::IFramebuffer>`.

1. Open [[Telescope]] in [[grep]] mode.
2. Search for `CFramebuffer`.
3. Press `CTRL`+`q` to add results to the Quickfix List
4. Press `:` and execute `:cfdo %s/CFramebuffer/SP<Render::IFramebuffer>/g | update`.

   > [!NOTE]
   > `cfdo` stands for:
   > `c` - Quickfix List
   > `f` - Files
   > `do` - Execute the following command

	> [!NOTE]
	> `| update` means that all the updated buffers should be saved to disk.