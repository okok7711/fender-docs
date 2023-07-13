---
layout: default
title: remove
parent: Value Operations
---

# `T remove(value: List, pos: Integer)`
Remove and return a value with a given position from `value`.
If `pos` not in `[0; value.len()[` or `pos` is not of type `Integer`, an error will be returned.

### Parameters
- `value`: list to remove from
- `pos`: index of the item to be removed

### Return Value
Returns the removed item from the list.

### Usage
```r
$test = [1, 2, 3]
remove(test, 0) # return 1
println(test) # output [2, 3], return [2, 3]
```

### Function Body
```rust
fndr_native_func!(
    /// Removes an element from a list and returns it or error if there is no element at that location
    remove_func,
    |_, mut value, pos| {
        let Int(pos) = *pos else {
            return Ok(FenderValue::make_error(format!("Remove must be indexed with an int: expected type `Int` found type `{}`", pos.get_type_id().to_string())).into());
        };
        Ok(match value.remove_at(pos) {
            Ok(v) => v,
            Err(s) => FenderValue::make_error(s).into(),
        })
    }
);
```