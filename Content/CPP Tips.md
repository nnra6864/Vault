---
created: 2026-08-16 08:29
tags:
  - note
  - dev/programming
  - dev/cpp
aliases:
---
`std::move()` can be used to assign a value without copying the heap memory.
Stack resources are still copied.
This is a great example from [[GPU Screen Recorder]]:
```cpp
        std::optional<Config> new_config = read_config(capture_options);
        if(new_config)
            config = std::move(new_config.value());
```
Instead of copying all of `new_config.value()` into `config`, it just transfers the ownership.