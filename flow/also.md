---
layout: default
title: also
parent: Flow
---

#### `T also(incoming_val: T, func: Function<T>)`
Runs `func(incoming_val)` and returns the original `incoming_val` without modification

##### Parameters
`incoming_val`: The argument to run `func` with and the return value of this function
`func`: The function to execute on `incoming_val`

##### Return Value
Returns `incoming_val`

##### Usage
```r
also(7, { 7 + 1 }) # return 7
```

##### Function Body
```rust
fndr_native_func!(
    /// Runs a function on a given value, then passes the original value
    also_func,
    |ctx, incoming_val, func| {
        type_match!(func {Function(f) => ctx.call(&f, vec![incoming_val.get_pass_object()])?});

        Ok(incoming_val)
    }
);
```
