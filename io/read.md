---
layout: default
title: read
parent: IO
---

# `String read(file_name: String)`
Read the contents of a file specified by the given file path into a string.

##### Parameters
`file_name`: represents the file path as a String from which to read the contents

##### Return Value
Either returns a String when the execution was successful, or a Fender Error type with the content `"failed to read file due to error: {e}"`  if there was an error whilst reading the file .

##### Usage
```r
$contents = read("test.txt") # return contents of test.txt
```

##### Function Body
```rust
fndr_native_func!(
    /// read in the contents of a file with given path
    read_func,
    |_, file_name| {
        let file_name = type_match!(file_name {(String(s))=>s});

        Ok(match std::fs::read_to_string(file_name.deref()) {
            Ok(s) => String(s.into()).into(),
            Err(e) => {
                FenderValue::make_error(format!("failed to read file due to error: {e}")).into()
            }
        })
    }
);
```
