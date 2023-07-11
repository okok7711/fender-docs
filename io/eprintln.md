---
layout: default
title: eprintln
parent: IO
---

#### `T eprintln(item: T, *argv: List<T>)`
Same as `println`, only that it prints to `stderr`
```rust
fndr_native_func!(
    /// Prints to stderr and adds a newline
    eprintln_func,
    |_, item, argv| {
        eprint!("{}", item.to_string());
        let argv = match &*argv {
            List(l) => l.deref(),

            e => unreachable!("Last argument is always a vararg list, found: {:?}", e),
        };
        for item in argv {
            eprint!("{}", item.to_string());
        }
        eprintln!();

        Ok(item)
    }
);
```
