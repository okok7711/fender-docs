---
layout: default
title: write
parent: IO
---

# `Bool write(data: T, file_name: String)`
Overwrites the file at `file_name` with stringified `data`

##### Parameters
`data`: data to overwrite the file with, is stringified when writing
`file_name`: represents the file path as a String where to write to

##### Return Value
Either returns a `true` Boolean value when the execution was successful, or a Fender Error type if there was an error whilst writing the file with the content `"failed to write file due to error: {e}"`.

##### Usage
```r
$success = write("new data", "test.txt") # return true
```

##### Function Body
```rust
fndr_native_func!(
    /// Overwrites file `file_name` with `data`
    write_func,
    |_, data, file_name| {
        let file_name = type_match!(file_name {(String(s))=>s});

        Ok(match std::fs::write(file_name.deref(), data.to_string()) {
            Ok(s) => Bool(true).into(),
            Err(e) => {
                FenderValue::make_error(format!("failed to write file due to error: {e}")).into()
            }
        })
    }
);
```
