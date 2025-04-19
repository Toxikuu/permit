# Permit

**Cute little rust library to permit a specific error.**

Lets you permit a specific error or permit conditionally for `Result<(), E>`.
Note that this does *not* work for `T`. This method may be chained.

**Examples**
```rust
use permit::Permit;

// Attempt to create a directory, but permit the case where it already exists.
if let Err(e) = std::fs::create_dir("/tmp/dir")
    .permit(|e| e.kind() == std::io::ErrorKind::AlreadyExists)
{
    // If a different error exists, handle it as usual.
    eprintln!("Failed to create /tmp/dir: {e:?}")
}
```

```rust
use permit::Permit;

// Alternative way to permit the case where a directory already exists.
let path = std::path::PathBuf::from("/tmp/dir");
std::fs::create_dir(&path).permit_if(path.exists()).unwrap_or_else(|e| {
    eprintln!("Failed to create {path:?}: {e:?}")
})
```

***Hint:** Look at the tests in `src/main.rs` for more usage examples.*
