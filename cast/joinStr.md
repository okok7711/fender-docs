---
layout: default
title: joinStr
parent: Cast Functions
---

# `String joinStr(value: R)`
Attempts to join members of `value` to a String

### Parameters
- `value`: value to be joined to a String

### Return Value
Returns the item joined to a String or a Fender Error if it can't join to String (e.g. Int).

### Usage
```r
$out = joinStr(['o', 'k', 'o', 'k']) # return "okok"
```

### Function Body
```rust
fndr_native_func!(
    /// Cast to a list
    join_to_string_func,
    |ctx, value| { Ok(value.join_to_string().into()) }
);
```




------