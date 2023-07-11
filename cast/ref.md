---
layout: default
title: ref
parent: Cast Functions
---

#### `Ref<T> ref(value: T)`
Wraps `value` in a `Ref`

##### Parameters
`item`: item to be wrapped in a `Ref`

##### Return Value
Returns the item wrapped in a `Ref`.

##### Usage
```r
$out = ref(0) # return Ref(0)
```

##### Function Body
```rust
fndr_native_func!(
    /// Wrap `value` in a `Ref`
    to_ref_func,
    |_, value| { Ok(Ref(InternalReference::new(value)).into()) }
);
```
