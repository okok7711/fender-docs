---
layout: default
title: len
parent: Value Operations
---

# `Integer len(item: T)`
Get the length of an iterable `item`.

### Parameters
- `item`: item to get the length of

### Return Value
Returns the length of the `item` or an Error with the content `Error("cannot get length of type {}")`

### Usage
```r
len([1, 2, 3]) # return 3
```

### Function Body
```rust
fndr_native_func!(
    /// Get the `FenderValue::len` of `item`
    len_func,
    |_, item| {
        Ok(match item.len() {
            Ok(len) => Int(len as i64),
            Err(e_str) => Error(e_str),
        }
        .into())
    }
);
```