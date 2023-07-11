---
layout: default
title: then
parent: Flow
---

# `Bool while(cond: Function, body: Function)`
Execute `body` while `cond` evaluates to `true`

##### Parameters
`cond`: Function to determine what to return
`body`: Function to run every iteration

##### Return Value
Returns `true` after finishing execution

##### Usage
```r
$x = 0
while({ x < 10 }, { x += 1 }) # return true
```

##### Function Body
```rust
fndr_native_func!(
    /// Execute `body` while `cond` evaluates to true
    ///
    /// `cond` and `body` must be of type `FenderValue::Function`
    ///
    /// `cond` must return type `FenderValue::Bool`
    while_func,
    |ctx, cond, body| {
        let (cond, body) = type_match!(cond, body{
            (Function(cond), Function(body)) => (cond, body)
        });

        loop {
            let call_res = ctx.call(&cond, Vec::with_capacity(0))?;
            let keep_run = type_match!( call_res {Bool(b) => b});

            if !keep_run {
                return Ok(Bool(true).into());
            } else {
                ctx.call(&body, Vec::with_capacity(0))?;
            }
        }
    }
);
```
