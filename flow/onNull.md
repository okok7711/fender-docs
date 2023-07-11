---
layout: default
title: onNull
parent: Flow
---

#### `T onNull(incoming_val: T, func: Function<T>)`
If the `incoming_val` is `Null` executes `func(incoming_val)`. Otherwise, returns `incoming_val` unchanged.

##### Parameters
`incoming_val`: incoming value to check for null, return value if not null
`func`: Function to evaluate if `incoming_val` is null

##### Return Value
Returns either `incoming_val` if it's not null, or `func(incoming_val)` if it is.

##### Usage
```r
onNull(null, { println("Is Null!") }) # returns "Is Null!"
```

##### Function Body
```rust
fndr_native_func!(
    /// If `incoming_val` is of type `FenderValue::Null` run `func`
    ///
    /// otherwise return `incoming_val`
    on_null_func,
    |ctx, incoming_val, func| {
        match incoming_val.unwrap_value() {
            Null => (),
            _ => return Ok(incoming_val),
        };

        type_match!(
            func {
                Function(f) => ctx.call(&f, Vec::new())
            }
        )
    }
);
```
