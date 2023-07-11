---
layout: default
title: shuffle
parent: Value Operations
---

# `List shuffle(list: List)`
Shuffles the members of a `List` object in place also returning it for chaining

##### Parameters
`list`: List in which the values should be swapped

##### Return Value
Returns the list after shuffling or an Error in case of a type mismatch.

##### Usage
```r
$test = [7, 7, 1, 1]
shuffle(test) # return the list shuffled (e.g. [1, 7, 7, 1])
println(test) # return and print the shuffled list
```

##### Function Body
```rust
fndr_native_func!(
    /// Shuffles a list in place, also returns the resulting list
    shuffle_func,
    |_, mut list| {
        Ok(match list.shuffle() {
            Ok(v) => list,
            Err(e) => FenderValue::make_error(e).into(),
        })
    }
);
```