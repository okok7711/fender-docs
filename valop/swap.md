---
layout: default
title: swap
parent: Value Operations
---

# `List swap(variable: List, pos_a: Integer, pos_b: Integer)`
Swaps to two values in a list specified by their positions.

### Parameters
- `variable`: List in which the values should be swapped
- `pos_a`: Position of the first value to be swapped
- `pos_b`: Position of the second value to be swapped

### Return Value
Returns the list after swapping the values or a Fender Error type if either `pos_a` or `pos_b` are not Integers or if `variable` is not a list.

### Usage
```r
swap([1, 2, 3], 0, 1) # return [2, 1, 3]
```

### Function Body
```rust
fndr_native_func!(
    /// Swap two values in a list
    swap_func,
    |_, mut variable, pos_a, pos_b| {
        let (Int(pos_a), Int(pos_b)) =  (&*pos_a, &*pos_b) else{
        return Ok(FenderValue::make_error(format!(
            "Swap indices must be of type Int, values provided were of following types (`{}`, `{}`)",
            pos_a.get_real_type_id().to_string(),
            pos_b.get_real_type_id().to_string(),
        ))
        .into());
    };

        Ok(match variable.deref_mut() {
            List(l) => {
                l.swap(*pos_a as usize, *pos_b as usize);
                List(l.to_vec().into()).into()
            }

            _ => Error(format!(
                "Cannot call swap on value of type `{}`",
                variable.get_real_type_id().to_string()
            ))
            .into(),
        })
    }
);
```