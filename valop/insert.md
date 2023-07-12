---
layout: default
title: dbg
parent: Value Operations
---

# `R insert(collection: <List, Map>, key: T, value: R)`
Insert `value` to `collection` at `key`. For `Map`s it will insert the key-value pair. For `List`s it will treat the given `key` as an index of where to insert to.

##### Parameters
`collection`: `List` or `Map` to insert into
`key`: Index or Key for the value to be inserted at
`value`: Value to insert into `collection` at `key`

##### Return Value
Returns the `value` which was previously assigned to `collection` at `key`.  
e. g.
```r
$test = [0, 1]
insert(test, 0, 2) # return 0, because the last item at index 0 in `test` was 0
```

##### Usage
```r
$test = [1, 2, 3]
insert(test, 0, 0) # return 1
println(test) # output [0, 1, 2, 3], return [0, 1, 2, 3]
```

##### Function Body
```rust
fndr_native_func!(
    /// Insert a key-value pair into a HashMap, or insert into a list at a given index
    insert_func,
    |_, mut collection, key, value| {
        let res = match collection.get_real_type_id() {
            FenderTypeId::HashMap => (*collection).insert(key.unwrap_value(), value),
            FenderTypeId::List if key.get_real_type_id() == FenderTypeId::Int => {
                collection.insert(key.unwrap_value(), value)
            }
            FenderTypeId::List => Err(format!(
                "Cannot index `List` with type `{}`",
                key.get_type_id().to_string()
            )),
            t => Err(format!("Cannot call `insert` on type `{}`", t.to_string())),
        };

        Ok(match res {
            Ok(v) => v,
            Err(e) => FenderValue::make_error(e).into(),
        })
    }
);
```