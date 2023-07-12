---
layout: default
title: rand
parent: Value Operations
---

# `Float rand()`
Get a random float in [0; 1]

##### Parameters

##### Return Value
Random float between 0 and 1

##### Usage
```r
rand() # return random float between 0 and 1, e.g. 0.0764449462357808
```

##### Function Body
```rust
fndr_native_func!(
    /// Get random float between 0 and 1
    rand_func,
    |_| { Ok(FenderValue::Float(rand::random()).into()) }
);
```