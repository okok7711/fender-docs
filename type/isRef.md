---
layout: default
title: isRef
parent: System
---

# `Bool isRef(value: T)`
Returns whether `value` is a `Reference`

### Parameters 
- `value`: value to check for reference

### Return Value
Returns `true` if `value` is a Reference and `false` otherwise.

### Usage
```r
isRef(ref(7)) # return true
```

### Function Body
```rust
fndr_native_func!(
    /// Returns `Bool(true)` if `value` is `FenderValue::Reference`, otherwise return `Bool(false)`
    is_ref_func,
    |_, value| { Ok(Bool(matches!(value.get_type_id(), FenderTypeId::Reference)).into()) }
);
```
