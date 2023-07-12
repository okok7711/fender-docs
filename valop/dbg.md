---
layout: default
title: dbg
parent: Value Operations
---

# `String dbg(value: T)`
Get a `String` that contains the debug text of the given `value`.
The debug text consists of the following pattern "<type>(<value>)"
For a `Char` type (e.g. `'a'`) the dbg representation would be `"Char('a')"`.
For a struct, this will include the struct name, the fields and their values.
```r
struct person{name, age}
$kloink = person(kloink, 19)
dbg(kloink) # returns "Struct(person{name:\"kloink\", age:19})"
```

##### Parameters
`value`: Value to get the debug text of

##### Return Value
Returns the debug text as a `String`

##### Usage
```r
dbg("My Value") # return "String(\"My Value\")"
```

##### Function Body
```rust
fndr_native_func!(
    /// Returns a `String` that contains the debug text of the given `value`
    dbg_func,
    |_, value| { Ok(FenderValue::make_string(value.fender_dbg_string()).into()) }
);
```