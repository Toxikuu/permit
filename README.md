# Permit it!

**Cute little rust library to permit errors**

Lets you permit a specific error or permit conditionally for `Result<(), E>`.
Note that this does *not* work for `T`. This method may be chained.

**Examples**
```rust
use permitit::Permit;

// Attempt to create a directory, but permit the case where it already exists.
if let Err(e) = std::fs::create_dir("/tmp/dir")
    .permit(|e| e.kind() == std::io::ErrorKind::AlreadyExists)
{
    // If a different error exists, handle it as usual.
    eprintln!("Failed to create /tmp/dir: {e:?}")
}
```

```rust
use permitit::Permit;

// Alternative way to permit the case where a directory already exists.
let path = std::path::PathBuf::from("/tmp/dir");
std::fs::create_dir(&path).permit_if(path.exists()).unwrap_or_else(|e| {
    eprintln!("Failed to create {path:?}: {e:?}")
})
```

***Hint:** Look at the tests in `src/lib.rs` for more usage examples.*
