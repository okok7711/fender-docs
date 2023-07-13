---
layout: default
title: list
parent: Cast Functions
---

# `List<T> list(value: R)`

Same as `int()` only with List return type.

### Parameters
- `value`: item to be cast to a List

### Return Value
Returns the item cast to a List or a Fender Error if it can't cast to List (e.g. Bool => List).
For strings, this will be a List of all the chars, for integer/float this will be a list with the only element being `value`

### Usage
```r
$out = list("okok") # return ["o", "k", "o", "k"]
```

### Function Body
```rust
fndr_native_func!(
    /// Cast to a list
    to_list_func,
    |ctx, value| {
        Ok(value
            .cast_to(crate::type_sys::type_id::FenderTypeId::List, ctx)?
            .into())
    }
);
```
