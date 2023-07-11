---
layout: default
title: float
parent: Cast Functions
---

# `Float float(item: T)`
Same as `int()` only with Float return type.

##### Parameters
`item`: item to be cast to a float

##### Return Value
Returns the item cast to a float or a Fender Error with content `"Invalid float string: {item}"` if attempting to cast from an invalid string

##### Usage
```r
$out = float("77.11") # return 77.11
```

##### Function Body
```rust
fndr_native_func!(
    /// Cast `FenderValue` to `FenderValue::Float`
    float_func,
    |_, item| {
        Ok(type_match!(
            item {
                String(s) => match s.parse(){
                    Ok(f) => Float(f).into(),
                    _ => FenderValue::make_error(format!("Invalid float string: {}", s.deref())).into()
                },
                Float(val) => Float(val).into(),
                Int(val) => Float(val as f64).into(),
                Bool(val) => Float(val as i8 as f64).into(),
                Char(val) => Float(val as u64 as f64).into()
            }
        ))
    }
);
```
