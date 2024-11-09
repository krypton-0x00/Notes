```rust
use chrono::prelude::*;

fn main() {
    let now: DateTime<Utc> = Utc::now();
    println!("Current time: {}", now);

    let date = NaiveDate::from_ymd(2024, 11, 1);
    println!("Specific date: {}", date);
}

```
