---
layout: default
title: apply
parent: Flow
---

#### `T apply(incoming_val: T, func: Function<T>)`
Runs `func(incoming_val)` and applies `func` to `incoming_val`.

##### Parameters
`incoming_val`: The argument to apply `func` to 
`func`: The function to apply to `incoming_val`

##### Return Value
Returns `incoming_val` after applying `func`

##### Usage
```r
apply(7, { 7 + 1 }) # return 8
```

##### Function Body
```rust
fndr_native_func!(
    /// Takes a value and a function and applies that function to the Runs a function on a given value, then passes the original value
    apply_func,
    |ctx, incoming_val, func| {
        let func = type_match!(func {Function(f) => f});

        ctx.call(&func, vec![incoming_val.get_pass_object()])
    }
);
```
