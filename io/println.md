---
layout: default
title: println
parent: IO
---

#### `T println(item: T, *argv: List<T>)`
Same as `print`, only appending an empty line print.
```rust
fndr_native_func!(
    /// Prints to stdout and adds a newline
    println_func,
    |_, item, argv| {
        print!("{}", item.to_string());
        let argv = match &*argv {
            List(l) => l.deref(),

            e => unreachable!("Last argument is always a vararg list, found: {:?}", e),
        };
        for item in argv {
            print!("{}", item.to_string());
        }
        println!();

        Ok(item)
    }
);
```
