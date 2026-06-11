---
created: 2026-06-09 12:09
tags:
  - note
  - tip
  - dev/programming
aliases:
---
## For loop

[[Zig]] doesn't use traditional `for` loops with 3 params, instead, it can iterate over collections, ranges etc.
Think of it more as a `foreach` loop in other languages.
Here's an example of accessing [[CLI]] args:
```zig
    const args = try init.minimal.args.toSlice(arena);
    for (args) |arg| {
        std.log.info("arg: {s}", .{arg});
    }
```
As you can see, the code is looping over the `args` slice and piping the current `arg`.

### Capturing the index

If you needed the index too, you could simply pipe it:
```zig
for (args, 0..) |arg, i| {
    std.log.info("Argument {d} is: {s}", .{i, arg});
}
```

> [!NOTE]
> `0..` is a range that starts at zero and ends at the length of args.

### Multi-sequence loops

Zig supports iterating over multiple sequences at once, as long as they are the same length:
```zig
const items = [_]u8{ 'A', 'B', 'C' };
const scores = [_]i32{ 10, 20, 30 };

for (items, scores) |item, score| {
    std.log.info("Item {c} scored {d} points", .{item, score});
}
```

### Ranges

You can also loop over ranges:
```zig
for (0..5) |i| {
    std.log.info("Number: {d}", .{i});
}
```

> [!NOTE]
> If you are searching for custom steps, such as incrementing by 2, take a look at [[#While Loop]]

## While loop

While loops are quite powerful in Zig, and cover everything from conditional expressions, to replacing traditional `for` loops that require different increments etc.
To make them even better, Zig added support for [[#Continuation Expression]], which allow you to execute something at the very end of the loop.
This is extremely useful for loops that might use `continue`, `break` etc., as it ensures the expression is always executed.

### Custom step

You could view this as a replacement of the traditional `for` loop.
Instead of simply iterating over a collection, it allows for custom conditions and expressions, such as incrementing by something other than `1`:
```zig
var i: usize = 0;
while (i < 10) : (i += 2) {

}
```

> [!NOTE]
> Everything after `:` is [[#Continuation Expression]].

## Continuation Expression

Continuation expression is an extremely powerful Zig feature.
It will always be executed, regardless of what happened in the body of the loop.
For example, it prevents an infinite [[#While loop]] in case we hit the continue branch:
```zig
var i: usize = 0;
while (i < 10) : (i += 1) {
    if (i % 2 == 0) {
        continue;
    }
    std.log.info("Odd number: {d}", .{i});
}
```
In a more traditional language, such as [[C-sharp]], you would have to handle incrementing in each branch, which is way more error prone and harder to maintain.

## Memory

Zig lets you manage memory completely manually and transparently, no hidden flow, allocations or de-allocations.
It has a few concepts to make this extremely easy and prevent memory leaks and errors.

### Arena

Think of the arena as a place you can allocate your memory to, and then de-allocate it all at once.
For example, if you are writing some text to a file, it's almost certain you won't need any of the allocated memory afterwards, so you might as well just de-allocate it in 1 line instead of 10.

> [!WARNING]
> If a functions uses its own local arena, and then returns something that was de-allocated by it, you'll run into issues.
> Recommended pattern to solve this issue is to pass the caller's allocator to the function, e.g:
> ```zig
> fn getBuffer(allocator: std.mem.Allocator) ![]u8 {
> 	const text_size = 100;
> 	const text_buffer = try allocator.alloc(u8, text_size);
> 	return text_buffer;
> }
> ```

### General Purpose Allocator

GPA is useful for allocating memory that doesn't get de-allocated as a part of a larger group.
Think of a bullet in a game, 10 could spawn and despawn at different times, so using an [[#Memory#Arena|Arena]] wouldn't make much sense.

### Fixed Buffer

If you are writing a super minimal app that you know won't exceed the pre-defined memory usage, you might want to consider using a Fixed Buffer.
It's great for embedded software use cases, such as on micro controllers, appliances etc., as it doesn't come with the cost of constantly allocating and de-allocating memory.

## Comments

Zig has 3 types of comments:
- [[#Comments#Normal|Normal]]
- [[#Comments#Doc|Doc]]
- [[#Comments#Top-Level Doc|Top-Level Doc]]

It supports only single line comments to allow for each line of code to be tokenized individually.

To generate the documentation from comments, run the following:
```sh
zig test -femit-docs main.zig
```
### Normal

Normal comments always start with `//` and are always ignored.

### Doc

Doc comment is used to document whatever follows it, and it always starts with exactly `///`.
If it's used in an invalid way, it will result in a compile error.
For example, you must not place them at the end of the file, in the middle of an expression, or above a non doc comment.

### Top-Level Doc

Top-Level Doc comments are used to document the current module, and always starts with `//!`.
It's a compile error if it's not placed at the start of a container, before any expressions.