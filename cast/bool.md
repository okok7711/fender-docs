---
layout: default
title: bool
parent: Cast Functions
---

# `Bool bool(item: T)`
Same as `int()` only with bool return type.

### Parameters
- `item`: item to be cast to a bool

### Return Value
Returns the item cast to a bool.

### Usage
```r
$out = bool(0) # return false
```

### Function Body
```rust
fndr_native_func!(
    /// Cast `FenderValue` to `FenderValue::String`
    to_bool_func,
    |_, item| { Ok(item.to_bool().into()) }
);
```
