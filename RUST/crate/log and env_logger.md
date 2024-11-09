```rust
use log::{info, warn};

fn main() {
    env_logger::init();
    info!("This is an informational message.");
    warn!("This is a warning!");
}

```