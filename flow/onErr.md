---
layout: default
title: onErr
parent: Flow
---

# `T onErr(incoming_val: T, func: Function<T>)`
If the `incoming_val` is of type `Error` executes `func(incoming_val)`. Otherwise, returns `incoming_val` unchanged.

##### Parameters
`incoming_val`: incoming value to check for an error, return value if not error
`func`: Function to evaluate if `incoming_val` is an error

##### Return Value
Returns either `incoming_val` if it's not an error, or `func(incoming_val)` if it is.

##### Usage
```r
onErr(1 - "1", { println("Error!") }) # returns "Error!"
```

##### Function Body
```rust
fndr_native_func!(
    /// If `incoming_val` is of type `FenderValue::Error` run `func`
    ///
    /// otherwise return `incoming_val`
    ///
    /// if `func` is called `incoming_val` will be passed to it
    on_err_func,
    |ctx, incoming_val, func| {
        match incoming_val.unwrap_value() {
            Error(e) => (),
            _ => return Ok(incoming_val),
        };

        type_match!(
            func{
                Function(f) => {
                    let args = if f.arg_count().max().unwrap_or(1) >= 1{
                        vec![incoming_val]
                    }else{
                        Vec::new()
                    };

                    ctx.call(&f, args)
                }
            }
        )
    }
);
```
