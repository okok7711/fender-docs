---
layout: default
title: then
parent: Flow
---

# `<Null|Bool> then(cond: Bool, body: Function)`
If `cond` is `true` body will be executed and `true` will be returned.

##### Parameters
`cond`: condition to determine if `body should be evaluated`
`body`: Function to evaluate and return if `cond` is `true`

##### Return Value
Return `true` if `cond` is `true`, return `Null` or `Error` if `cond` is `Null` or `Error` respectively. Return `Null` if `cond` is false

##### Usage
```r
then(true, { "this will be run" }) # return true
```

##### Function Body
```rust
fndr_native_func!(
    /// Conditional execution
    ///
    /// Executes `body` if `cond` is equivalent to `true`
    ///
    /// `body` must be of type `FenderValue::
    then_func,
    |ctx, cond, body| {
        if matches!(*cond, FenderValue::Null | FenderValue::Error(_)) {
            return Ok(cond);
        }

        if let FenderValue::Bool(false) = &*cond {
            return Ok(Null.into());
        }

        Ok(match &*body {
            Function(f) => {
                ctx.call(f, Vec::with_capacity(0))?;
                Bool(true)
            }
            b => Error(format!(
                "then body expected, found {:?}; else will be run",
                b.get_type_id()
            )),
        }
        .into())
    }
);
```
