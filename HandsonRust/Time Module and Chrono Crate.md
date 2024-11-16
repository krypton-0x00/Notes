# Built in std::time
```rust
use std::{
    ops::Sub,
    time::{Duration, Instant},
};

pub fn test_stdtime() {
    let ten_sec = Duration::from_secs(10);
    println!("{:?}", { ten_sec.as_secs() });

    let five_sec: Duration = Duration::from_secs(5);
    let hundred_sec: Duration = Duration::from_secs(100);

    let dur3 = ten_sec.sub(five_sec);
    let checkedDur3 = ten_sec.checked_sub(hundred_sec);

    println!("{}", dur3.as_secs()); //will panic if it returns -ve values
    println!("{}", checkedDur3.unwrap_or_default().as_secs()); //will not panic on -ve values

    //USING INSTANT TO CALCULATE TIME
    let now: Instant = Instant::now();
    std::thread::sleep(Duration::from_millis(200));
    let mut count: i64 = 0;
    for i in 0..100000000 {
        count += 1;
    }

    println!("{}", now.elapsed().as_millis())
}

fn main() {
    test_stdtime();
}
```


# Chrono crate
