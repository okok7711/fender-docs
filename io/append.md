---
layout: default
title: append
parent: IO
---

# `Bool append(data: T, file_name: String)`
Appends to the file at `file_name` with stringified `data`.

##### Parameters
`data`: data to append to the file with, is stringified when writing
`file_name`: represents the file path as a String from which to append the contents to

##### Return Value
Either returns a `true` Boolean value when the execution was successful, or a Fender Error type if there was an error whilst writing the file with the content `"failed to append to file due to error: {e}"`. If there was an error opening the file, Fender Error with content `"failed to open file due to error: {e}"` is returned

##### Usage
```r
$success = append(" newer data", "test.txt") # return true
```

##### Function Body
```rust
fndr_native_func!(
    /// Appends to file `file_name` with `data`
    append_func,
    |_, data, file_name| {
        let file_name = type_match!(file_name {(String(s))=>s});

        let mut file = match std::fs::OpenOptions::new()
            .write(true)
            .append(true)
            .create(true)
            .open(file_name.deref())
        {
            Ok(f) => f,
            Err(e) => {
                return Ok(FenderValue::make_error(format!(
                    "failed to open file due to error: {e}"
                ))
                .into())
            }
        };

        Ok(match write!(file, "{}", data.to_string()) {
            Ok(s) => Bool(true).into(),
            Err(e) => {
                FenderValue::make_error(format!("failed to append to file due to error: {e}"))
                    .into()
            }
        })
    }
);
```
