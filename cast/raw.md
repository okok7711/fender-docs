---
layout: default
title: raw
parent: Cast Functions
---

# `T raw(item: T)`
Unwraps all references, returns a raw value of type `T`.

### Parameters
- `item`: item to unwrap

### Return Value
Returns the unwrapped item.

### Usage
```r
$unwrapped = raw(ref(0)) # return 0
```

### Function Body
```rust
fndr_native_func!(
    /// return a raw `FenderValue` unwrapping all references
    get_raw_func,
    |_, item| Ok(item.unwrap_value().into())
);
```
