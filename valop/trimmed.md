---
layout: default
title: trim
parent: Value Operations
---

# `String trimmed(string: String, ignore_list: List<Char>?)`
Trim leading and trailing characters that match `ignore_list` or `DEFAULT_TRIM_SET`.  
`const DEFAULT_TRIM_SET: &[char] = &['\t', ' ', '\n', '\r']`  
If `ignore_list` is not provided, `DEFAULT_TRIM_SET` will be used.  
Unlike [`String trim(string: String, ignore_list: List<Char>?)`](../valop/trim) this does not modify `string`.

### Parameters
- `string`: string to trim characters from.
- `ignore_list`: characters to trim, if not provided `['\t', ' ', '\n', '\r']` will be trimmed

### Return Value
Returns the trimmed string.

### Usage
```r
$test = " i like spaces! "
trim(test) # return "i like spaces!"
println(test) # output " i like spaces! ", return " i like spaces! "
```

### Function Body
```rust
fndr_native_func!(
    /// Trim leading and trailing characters that match `ignore_list` or `DEFAULT_TRIM_SET`
    ///
    /// this modifies incoming `string` and returns it
    trim_func,
    |_, mut string, ignore_list| {
        const DEFAULT_TRIM_SET: &[char] = &['\t', ' ', '\n', '\r'];

        type_match!(
            string, ignore_list {
            (String(mut s), List(mut l)) => {

                let char_list = l.iter().filter_map(|v| v.to_string().chars().next()).collect::<Vec<_>>();
                *s = s.trim_matches(&char_list[..]).to_owned();
            },
            (String(mut s), Char(c)) => *s = s.trim_matches(c).to_owned(),
            (String(mut s), String(ignore_s)) => *s = s.trim_matches(&ignore_s.chars().collect::<Vec<_>>()[..]).to_owned(),
            (String(mut s), Null) => *s = s.trim_matches(DEFAULT_TRIM_SET).to_owned()

        });

        Ok(string)
    }
);
```