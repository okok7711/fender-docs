---
layout: default
title: readLine
parent: IO
---

# `String readLine()`
Read a line from `stdin`. Function waits for user input and returns input line as String.

##### Return Value
Either returns a String when the execution was successful, or a Fender Error type if there was an error whilst reading from `stdin`

##### Usage
```r
$input = readLine() # return user input
```

##### Function Body
```rust
match std::io::stdin().lines().next() {
    Some(Ok(s)) => Ok(String(s.into()).into()),
    Some(Err(e)) => Ok(FenderValue::make_error(e.to_string()).into()),
    None => Ok(FenderValue::make_error("End of file".to_string()).into()),
}

```
 