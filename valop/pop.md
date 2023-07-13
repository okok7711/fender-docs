---
layout: default
title: push
parent: Value Operations
---

# `T pop(list: List)`
Pop a value off the end of `list`, as a result that value will be removed from the list

### Parameters
- `list`: list to pop from

### Return Value
Returns the last item of the array, `null` if `list.len() == 0`

### Usage
```r
$test = [1, 2, 3]
pop(test) # return 3
```

### Function Body
```rust
fndr_native_func!(
    /// Pops a value off the end of a list
    pop_func,
    |_, mut list| {
        Ok(match list.pop() {
            Ok(v) => v,
            Err(e) => FenderValue::make_error(e).into(),
        })
    }
);
```