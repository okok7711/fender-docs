---
layout: default
title: removePass
parent: Value Operations
---

# `List removePass(value: List, pos: Integer)`
Remove a value with a given position from `value`.
If `pos` not in `[0; value.len()[` or `pos` is not of type `Integer`, an error will be returned.
Differs from [`T remove(value: List, pos: Integer)`](../valop/remove) by returning the resulting list instead of the removed item.

### Parameters
- `value`: list to remove from
- `pos`: index of the item to be removed

### Return Value
Returns the list with the item removed

### Usage
```r
$test = [1, 2, 3]
removePass(test, 0) # return [2, 3]
println(test) # output [2, 3], return [2, 3]
```

### Function Body
```rust
fndr_native_func!(
    /// Removes an element from a list and returns the list it was removed from
    remove_pass_func,
    |_, mut value, pos| {
        let Int(pos) = *pos else {
            return Ok(FenderValue::make_error(format!("Remove must be indexed with an int: expected type `Int` found type `{}`", pos.get_type_id().to_string())).into());
        };
        Ok(match value.remove_at(pos) {
            Ok(_) => value,
            Err(s) => FenderValue::make_error(s).into(),
        })
    }
);
```