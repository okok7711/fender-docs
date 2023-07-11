---
layout: default
title: if
parent: Flow
---

# `<T|R> if(cond: Bool, if_true: T|Function<T>, if_false: R|Function<R>)`
If `cond` is true, will return `if_true` if it's a value, otherwise it will evaluate and return its return value, if `cond` is false, the same will happen for `if_false`

##### Parameters
`cond`: condition to determine what to return
`if_true`: Function or value to evaluate and return if `cond` is truthy
`if_false`: Function or value to evaluate and return if `cond` is falsey

##### Return Value
Returns either `if_true` or `if_false` or their return values when they are functions

##### Usage
```r
$temp = 7
if(temp == 7, "Match!", "No Match :(") # return "Match!"
```

##### Function Body
```rust
fndr_native_func!(
    /// if `cond` is true will evaluate and return `if_true` else will evaluate and return `if_false
    if_func,
    |ctx, cond, if_true, if_false| {
        let cond = type_match!( cond {Bool(b) => b});

        if cond {
            Ok(match &*if_true {
                Function(if_true) => ctx.call(if_true, Vec::with_capacity(0))?,
                v => v.clone().into(),
            })
        } else {
            Ok(match &*if_false {
                Function(if_false) => ctx.call(if_false, Vec::with_capacity(0))?,
                v => v.clone().into(),
            })
        }
    }
);
```
