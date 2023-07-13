---
layout: default
title: shell
parent: System
---

# `String shell(cmd: T, cmd_name: String?, shell_path: String?)`
Execute a command in the host system shell invoked by `shell_path`.  
If `shell_path` is not set, `DEFAULT_SHELL` will be used. `DEFAULT_SHELL` is equal to `cmd /c` on Windows and `sh -c` on all other Platforms.  
The command that will be executed depends on the amount of arguments supplied to the function.  
The command can be expressed as `{shell_path or DEFAULT_SHELL} "{cmd_name} {cmd}"`, that means that you can chain the output of previous functions as the input of executed shell functions.  
e.g.
```r
"hello world".shell("echo -n") # return "hello world", execute "echo -n hello world"
```
you can however also supply arguments without supplying `cmd_name` by putting them into the `cmd`.  
e.g.
```r
shell("echo -n hello world") # return "hello world", execute "echo -n hello world"
```
Both snippets work the same here.

### Parameters
- `cmd`: command to execute if no other args supplied **OR** argument to supply to `cmd_name`
- `cmd_name`: command to execute, optional, if not specified `cmd` will be executed
- `shell_path`: shell to invoke 

### Return Value
Returns all of `stdout` in a `String` if the command succeeds, if it fails returns all of `stderr`

### Usage
```r
shell("echo -n test") # return "test"
```

### Function Body
```rust
fndr_native_func!(
    /// Interact with host system shell
    ///
    /// Commands run through `shell` will return all of stdout in a `FenderValue::String` on
    /// success, on failure all of stderr will be returned in a `FenderValue::Error`
    ///
    /// `shell` can operate with 1 to 3 arguments
    ///
    /// One argument will be equivalent to just passing that text into your shell
    /// ```fender
    /// shell("echo hello world > test.txt")
    /// ```
    ///
    /// Two arguments allow you to take in a specific command after receiving arguments that will be passed to the command.
    /// This can be useful to use a shell command in the middle of a function chain
    /// ```fender
    /// $wcResults = readLine().shell("wc").else({println(cannot get file); "0"})
    /// ```
    ///
    /// The default shell command for *nix is `sh -c` and for windows is `batch -c`, if you wish to use a different shell that can
    /// be passed as a third argument
    ///
    /// ```fender
    /// "hello".shell("echo -n", "fish -c")
    /// ```
    shell_func,
    |_, cmd, cmd_name, shell_path| {
        #[cfg(target_os = "windows")]
        const DEFAULT_SHELL: &str = "cmd /c";
        #[cfg(not(target_os = "windows"))]
        const DEFAULT_SHELL: &str = "sh -c";

        let (mut shell, cmd) = match (&*cmd_name, &*shell_path) {
            (String(name), String(shell)) => (
                shell.split(' '),
                format!("{} {}", name.deref(), cmd.to_string()),
            ),
            (String(name), Null) => (
                DEFAULT_SHELL.split(' '),
                format!("{} {}", name.deref(), cmd.to_string()),
            ),
            (Null, Null) => (DEFAULT_SHELL.split(' '), cmd.to_string()),
            _ => {
                return Ok(FenderValue::make_error(format!("unexpected arg types for shell command, expected `(Any, String?, String?)` found `({:?}, {:?}, {:?})", cmd.get_type_id(), cmd_name.get_type_id(), shell_path.get_type_id())).into());
            }
        };

        let shell_name = shell.next().unwrap_or_default();

        let result = match Command::new(shell_name).args(shell).arg(&cmd).output() {
            Ok(v) => v,
            Err(e) => {
                return Ok(FenderValue::make_error(format!(
                    "failed to run command {:?} {:?}\t{}",
                    cmd_name,
                    cmd,
                    e.to_string().trim()
                ))
                .into())
            }
        };

        let output = {
            let output = if result.status.success() {
                result.stdout
            } else {
                result.stderr
            };

            match std::string::String::from_utf8(output) {
                Ok(v) => v,
                Err(_) => "Failed to read command output".into(),
            }
        };

        Ok(if result.status.success() {
            String(output.into())
        } else {
            Error(output)
        }
        .into())
    }
);
```
