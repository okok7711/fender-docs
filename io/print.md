---
layout: default
title: print
parent: IO
---

#### `T print(item: T, *argv: List<T>)`
Prints text to `stdout`, prints `item` and then iterates over `argv`, printing each member sequentially. All of the `argv` members are stringified.

##### Parameters
`item`: Item to be printed to stdout.  Will be the return value.
`argv`: argument vector of other values to print. 

##### Return Value
Returns the `ìtem` parameter.

##### Usage
```r
print("test", 1, 2, 3) # return "test"
```

##### Function Body
```rust
fndr_native_func!(
    /// Prints to stdout
    print_func,
    |_, item, argv| {
        print!("{}", item.to_string());
        let argv = match &*argv {
            List(l) => l.deref(),
            e => unreachable!("Last argument is always a vararg list, found: {:?}", e),
        };
        for item in argv {
            print!("{}", item.to_string());
        }
        Ok(item)
    }
);
```