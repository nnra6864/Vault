---
created: 2026-06-13 16:36
tags:
  - note
  - tip
  - guide
  - 3d_print
aliases:
---
This is almost certainly caused by a somewhat hidden, enabled by default, [[Orca Slicer]] setting.
1. Enable `Process -> Show/Hide advanced parameters`
	[[Orca_Slicer_-_Advanced_Parameters_Toggle.jxl]]
2. Disable `Others -> G-code output -> Reduce infill retraction`
	[[Orca_Slicer_-_Reduce_infill_retraction.jxl]]

Test to confirm it no longer scrapes the infill.