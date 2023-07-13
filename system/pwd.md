---
layout: default
title: shell
parent: System
---

# `String pwd()`
Get the currrent working directory. For *nix systems this is equivalent to [`shell("pwd")`](../shell)

### Parameters 

### Return Value
returns current working directory as a `String`

### Usage
```r
pwd() # return "/mnt/z/gits/fender-docs" (current working directory)
```

### Function Body
```rust
fndr_native_func!(
    /// Get the current working directory
    ///
    /// This is equivalent to `shell("pwd")` on *nix systems
    pwd_func,
    |_| {
        Ok(match std::env::current_dir() {
            Ok(path) => FenderValue::make_string(path.to_string_lossy().into()).into(),
            Err(e) => FenderValue::make_error(format!("failed to get current path: {}", e)).into(),
        })
    }
);
```
