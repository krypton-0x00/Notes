## [A note on printing performance](https://rust-cli.github.io/book/tutorial/output.html#a-note-on-printing-performance)

Printing to the terminal is surprisingly slow! If you call things like `println!` in a loop, it can easily become a bottleneck in an otherwise fast program. To speed this up, there are two things you can do.

First, you might want to reduce the number of writes that actually “flush” to the terminal. `println!` tells the system to flush to the terminal _every_ time, because it is common to print each new line. If you don’t need that, you can wrap your `stdout` handle in a [`BufWriter`](https://doc.rust-lang.org/1.39.0/std/io/struct.BufWriter.html) which by default buffers up to 8 kB. (You can still call `.flush()` on this `BufWriter` when you want to print immediately.)

```rust

#![allow(unused)]
fn main() {
use std::io::{self, Write};

let stdout = io::stdout(); // get the global stdout entity
let mut handle = io::BufWriter::new(stdout); // optional: wrap that handle in a buffer
writeln!(handle, "foo: {}", 42); // add `?` if you care about errors here
}

```

Second, it helps to acquire a lock on `stdout` (or `stderr`) and use `writeln!` to print to it directly. This prevents the system from locking and unlocking `stdout` over and over again.

```rust


#![allow(unused)]
fn main() {
use std::io::{self, Write};

let stdout = io::stdout(); // get the global stdout entity
let mut handle = stdout.lock(); // acquire a lock on it
writeln!(handle, "foo: {}", 42); // add `?` if you care about errors here
}


```

You can also combine the two approaches.