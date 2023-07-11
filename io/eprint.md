---
layout: default
title: eprint
parent: IO
---

# `T eprint(item: T, *argv: List<T>)`
Same as `print`, only that it prints to `stderr`
```rust
fndr_native_func!(
    /// Prints to stderr
    eprint_func,
    |_, item, argv| {
        eprint!("{}", item.to_string());
        let argv = match &*argv {
            List(l) => l.deref(),
            e => unreachable!("Last argument is always a vararg list, found: {:?}", e),
        };
        for item in argv {
            eprint!("{}", item.to_string());
        }
        Ok(item)
    }
);
```
