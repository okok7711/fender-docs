---
layout: default
title: strToType
parent: System
---

# `T strToType(value: String)`
Get the type from a string representing that type

This is the same string that is also the first part of [`dbg(value)`](../../valop/dbg)

### Parameters 
- `value`: String representation of `T`

### Return Value
Returns the type representated by `value`

### Usage
```r
strToType("Int") # return Int
```

### Function Body
```rust
fndr_native_func!(
    /// Get the `FenderValue::Type` from a string representing that type
    type_from_name_func,
    |_, value| {
        let value_str = type_match!(
            value {
                (String(v)) => v
            }
        );

        Ok(match FenderTypeId::type_from_str(&value_str) {
            Some(t) => Type(t),
            None => {
                FenderValue::make_error(format!("No type found matching name `{}`", *value_str))
            }
        }
        .into())
    }
);
```
