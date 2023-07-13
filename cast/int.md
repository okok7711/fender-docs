---
layout: default
title: int
parent: Cast Functions
---

# `Integer int(item: T)`
Cast a Value to an Integer.

### Parameters
- `item`: item to cast to an integer

### Return Value
Returns the item cast to an integer or a Fender Error with content `"Invalid int string: {item}"` if attempting to cast from an invalid string

### Usage
```r
$out = int("7711") # return 7711
```

### Function Body
```rust
fndr_native_func!(
    /// Cast `FenderValue` to `FenderValue::Int`
    int_func,
    |_, item| {
        Ok(type_match! (
            item {
                String(s) => match s.parse() {
                    Ok(i) => Int(i).into(),
                    _ => FenderValue::make_error(format!("Invalid int string: {}", s.deref())).into(),
                },
                Int(val) => Int(val).into(),
                Float(val) => Int(val as i64).into(),
                Bool(val) => Int(val as i64).into(),
                Char(val) => Int(val as i64).into()
            }
        ))
    }
);
```
