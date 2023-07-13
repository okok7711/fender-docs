---
layout: default
title: type
parent: System
---

# `T type(value: T)`
Get the type of `value`

### Parameters 
- `value`: Value to get the type of

### Return Value
Returns the type of `value`

### Usage
```r
type(1) # return Int
```

### Function Body
```rust
fndr_native_func!(
    /// Get the `type` of `value`
    get_type_func,
    |_, value| { Ok(Type(value.get_real_type_id()).into()) }
);
```
