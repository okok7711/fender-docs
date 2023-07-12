---
layout: default
title: else
parent: Flow
---

# `T else(cond: Bool, body: <T, Function<T>>)`
If `cond` is one of `null`, `Error` or `false`, it will evaluate and return `body`

##### Parameters
`cond`: condition to determine if to evaluate `body`
`body`: Function or value to evaluate and return if `cond` is one of `null`, `Error` or `false`

##### Return Value
Returns either `body` or its return value when it's a function.

##### Usage
```r
else(null, "This will be shown!") # return "Whis will be shown!"
```

##### Function Body
```rust
fndr_native_func!(
    /// If called on `FenderValue::Null`, `FenderValue::Error`, or `FenderValue::Bool(false)` will evaluate and return `body`
    else_func,
    |ctx, cond, body| {
        if !matches!(*cond, FenderValue::Null | FenderValue::Error(_)) {
            return Ok(cond);
        }

        Ok(match &*body {
            Function(f) => ctx.call(f, Vec::with_capacity(0))?,
            _ => body,
        })
    }
);
```



