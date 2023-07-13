---
layout: default
title: cd
parent: System
---

# `Bool cd(path: String)`
Change the current working directory (pwd) to `path` 

### Parameters 
- `path`: path to change the working directory to

### Return Value
returns `true` if the operation succeeded and an Error if it didn't

### Usage
```r
pwd() # return "/mnt/z/gits/fender-docs" (current working directory)
cd("..") # return true
pwd() # return "/mnt/z/gits" (current working directory)
```

### Function Body
```rust
fndr_native_func!(
    /// Change the current working directory environment variable (PWD)
    cd_func,
    |_, path| {
        let path = type_match!(path {String(s) => s.to_string()});

        Ok(match std::env::set_current_dir(path) {
            Ok(path) => Bool(true).into(),
            Err(e) => {
                FenderValue::make_error(format!("failed to change current directory: {}", e)).into()
            }
        })
    }
);
```
