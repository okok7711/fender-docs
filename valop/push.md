---
layout: default
title: push
parent: Value Operations
---

# `List push(list: List, value: T)`
Push `value` to the end of a given list, also returning the modified list.

### Parameters
- `list`: list to push the value to
- `value`: value to push to the list

### Return Value
Returns the list with the value appended to it

### Usage
```r
$test = [1, 2, 3]
push(test, 4) # return [1, 2, 3, 4]
println(test) # output [1, 2, 3, 4], return [1, 2, 3, 4]
```

### Function Body
```rust
fndr_native_func!(
    /// Pushes a value to the end of a list
    push_func,
    |_, mut list, value| {
        Ok(match list.push(value) {
            Ok(_) => list,
            Err(e) => FenderValue::make_error(e).into(),
        })
    }
);
```