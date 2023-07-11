---
layout: default
title: getShuffled
parent: Value Operations
---

#### `List getShuffled(list: List)`
Creates and returns a copy of the given list shuffled without modifying the original list

##### Parameters
`variable`: List in which the values should be swapped
`pos_a`: Position of the first value to be swapped
`pos_b`: Position of the second value to be swapped

##### Return Value
Returns the list after swapping the values or a Fender Error type if either `pos_a` or `pos_b` are not Integers or if `variable` is not a list.

##### Usage
```r
swap([1, 2, 3], 0, 1) # return [2, 1, 3]
```

##### Function Body
```rust
fndr_native_func!(
    /// Creates, and returns, a copy of the given list shuffled,
    /// without modifying the original list
    get_shuffled_func,
    |_, mut list| {
        Ok(match list.get_shuffled() {
            Ok(v) => v.into(),
            Err(e) => FenderValue::make_error(e).into(),
        })
    }
);
```
