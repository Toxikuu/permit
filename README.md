# Permit

**Cute little rust library to permit a specific error.**

Lets you permit a specific error for `Result<(), E>`. Note that this does *not*
work for `T`. This method may be chained.

**Examples**
```rust
// Attempt to create a directory, but permit the case where it already exists
if let Err(e) = std::fs::create_dir("/tmp/dir")
    .permit(|e| e.kind() == std::io::ErrorKind::AlreadyExists)
{
    // If a different error exists, handle it as usual
    eprintln!("Failed to create /tmp/dir: {e}")
}
```

***Hint:** Look at the tests in `src/main.rs` for more usage examples.*
