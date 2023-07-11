---
layout: default
title: raw
parent: Cast Functions
---

#### `String str(item: T)`
Same as `int()` only with String return type.

##### Parameters
`item`: item to be cast to a string

##### Return Value
Returns the item cast to a string.

##### Usage
```r
$out = str(77.11) # return "77.11"
```

##### Function Body
```rust
fndr_native_func!(
    /// Cast `FenderValue` to `FenderValue::String`
    str_func,
    |_, item| { Ok(String(item.to_string().into()).into()) }
);
```
