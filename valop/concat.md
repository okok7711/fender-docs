---
layout: default
title: concat
parent: Value Operations
---

# `<List, String> concat(a: <List, String>, b: <List, String>)`
Concatenate `b` to `a`. Both `a` and `b` need to have the same type but can be either `List`s or `String`s. For `String`s you can also use the shorthand `+` operator for concatenation.

##### Parameters
`a`: `List` or `String` to concat `b` to
`b`: `List` or `String` to concat to `a`

##### Return Value
Returns the concatenation of `a` and `b`

##### Usage
```r
$a = [1, 2, 3]
$b = [4, 5, 6]

concat(a, b) # return [1, 2, 3, 4, 5, 6]
```

##### Function Body
```rust
fndr_native_func!(
    /// Returns and String/List with the combined String/List
    concat_func,
    |_, a, b| {
        type_match!(
            a, b {
                (String(a), String(b)) => Ok(FenderValue::make_string(format!("{}{}", *a, *b)).into()),
                (List(a), List(b)) => {
                    let mut new_list = a.to_vec();
                    new_list.extend(b.iter().cloned());
                    Ok(FenderValue::make_list(new_list).into())
                }
            }
        )
    }
);
```