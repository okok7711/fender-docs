---
layout: default
title: onType
parent: Flow
---

# `T onType(incoming_val: T, type_id: R, func: Function<T>)`
If the `incoming_val` is of type `type_id` executes and returns `func(incoming_val)`. Otherwise, returns `incoming_val` unchanged.

##### Parameters
`incoming_val`: incoming value to check for type `type_id`, return value if not of type `type_id`
`func`: Function to evaluate if `incoming_val` is of type `type_id`

##### Return Value
Returns either `incoming_val` if it is not of type `type_id`, or `func(incoming_val)` if it is.

##### Usage
```r
onType(7, Int, (x) { x + 1 }) # returns 8
```

##### Function Body
```rust
fndr_native_func!(
    /// If `incoming_val` is of type `type_id` run `func`
    ///
    /// otherwise return `incoming_val`
    ///
    /// if `func` is called `incoming_val` will be passed to it
    on_type_func,
    |ctx, incoming_val, type_id, func| {
        type_match!(
            type_id, func{
                (Type(ty), Function(f)) => {
                    if incoming_val.get_real_type_id() == ty {
                        let args = if f.arg_count().max().unwrap_or(1) >= 1{
                            vec![incoming_val]
                        }else{
                            Vec::new()
                        };
                        ctx.call(&f, args)
                    }else{
                        Ok(incoming_val)
                    }
                }
            }
        )
    }
);
```
