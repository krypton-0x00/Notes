![[Pasted image 20241120133224.png]]
```rust
use std::thread::{spawn, JoinHandle};
fn test_thread() {
    let mut x: u128 = 0u128;
    for i in 0..500_000_000 {
        x += i;
    }
}
fn spawn_thread() {
    println!("Starting new worker thread");
    let thread_fn = || {
        let mut x: u128 = 0u128;
        for i in 0..500_000_000 {
            x += i;
        }
        println!("{x}")
    };
    println!("Completed");
    //spawing 2 worker threads threads
    let handle: JoinHandle<()> = spawn(thread_fn);

    let handle2: JoinHandle<()> = spawn(thread_fn);
    //tell the main thread that you keep doing this work,
    test_thread();

    //tell main thread to stop exec if you have finished and wait for the worker threads to finish.
    handle.join(); // tells the main  thread to stop until worker thread is complted

    handle2.join(); // tells the main  thread to stop until worker thread is complted
}
fn main() {
    spawn_thread();
}
```
## Or we can loop till the worker threads are done that will also keep the main thread busy

```rust
use std::thread::{spawn, JoinHandle};
fn test_thread() {
    let mut x: u128 = 0u128;
    for i in 0..500_000_000 {
        x += i;
    }
}
fn spawn_thread() {
    println!("Starting new worker thread");
    let thread_fn = || {
        let mut x: u128 = 0u128;
        for i in 0..500_000_000 {
            x += i;
        }
        println!("{x}")
    };
    println!("Completed");
    //spawing 2 worker threads threads
    let handle: JoinHandle<()> = spawn(thread_fn);

    let handle2: JoinHandle<()> = spawn(thread_fn);

    loop {
        test_thread();
        if handle.is_finished() && handle2.is_finished() {
            println!("All the workers are done");
            break;
        }
    }
}
fn main() {
    spawn_thread();
}
```

# Passing Data from outer scope (Scoped threads)

```rust
use std::thread::spawn;

struct Person {
    name: String,
}

fn test_thread_variables() {
    let age: u8 = 34;
    let person1 = Person {
        name: "Shakir".to_string(),
    };
    let print_age = move || {
        println!("you are : {} and your age is :{}", person1.name, age);
    };

    spawn(print_age).join();
    println!("Worker thread done.")
}

fn main() {
    test_thread_variables();
}
```

But if we try to use that data after the closure
ERROR:
```rust
use std::thread::spawn;

struct Person {
    name: String,
}

fn test_thread_variables() {
    let age: u8 = 34;
    let person1 = Person {
        name: "Shakir".to_string(),
    };
    let print_age = move || {
        println!("you are : {} and your age is :{}", person1.name, age);
    };

    println!("you are : {} and your age is :{}", person1.name, age); //error Borrow of moved value

    spawn(print_age).join();
    println!("Worker thread done.")
}

fn main() {
    test_thread_variables();
}
```

# Fix (Scoped Threads) 
```rust
use std::thread::{scope, spawn};

struct Person {
    name: String,
}

fn test_thread_variables() {
    let age: u8 = 34;
    let person1 = Person {
        name: "Shakir".to_string(),
    };
    let print_age = || {
        println!("you are : {} and your age is :{}", &person1.name, age);
    };

    //spawn(print_age).join();
    
    scope(|scope| {
        scope.spawn(print_age);
        //scopes by default join the thread we dont have to do it manually
    });

    println!("you are : {} and your age is :{}", person1.name, age);

    println!("Worker thread done.")
}

fn main() {
    test_thread_variables();
}
```

# Mutex (Share rust thread data)
```rust
use std::{ops::AddAssign, sync::Mutex};

fn test_mutex() {
    let mut score: Mutex<u16> = Mutex::new(0u16);

    let unlocked_data = score.lock(); //Mutex guard
    let mut data = unlocked_data.unwrap(); //Data ie: 0

    println!("{}", data); //0
    data.add_assign(5); //adds 5 and assigns back to mutex
    println!("{}", data);
    
    drop(data); //drop
}

fn main() {
    test_mutex();
}
```

### With threads.

```rust
use std::{
    ops::AddAssign,
    sync::Mutex,
    thread::{scope, spawn},
};

fn test_mutex() {
    let mut score: Mutex<u16> = Mutex::new(0u16);

    let myfunc = || {
        let mut data = score.lock().unwrap();
        for i in 1..10 {
            data.add_assign(i);
            println!("Thread 1 is adding {}", i);
        }
    }; //mutex guard will get outof scope and returns contorl back to the caller.
    let myfunc2 = || {
        println!("T2, Wating for mutex lock");
        let mut data = score.lock().unwrap();
        for i in 1..10 {
            data.add_assign(i);
            println!("Thread 2 is adding, {}", i);
        }
    };

    _ = scope(|s| {
        s.spawn(myfunc);
        s.spawn(myfunc2);
    });
    println!("{:?}", score.lock().unwrap());
}

fn main() {
    test_mutex();
}
```



```output
Thread 1 is adding 1
Thread 1 is adding 2
Thread 1 is adding 3
Thread 1 is adding 4
Thread 1 is adding 5
Thread 1 is adding 6
Thread 1 is adding 7
Thread 1 is adding 8
Thread 1 is adding 9
T2, Wating for mutex lock
Thread 2 is adding, 1
Thread 2 is adding, 2
Thread 2 is adding, 3
Thread 2 is adding, 4
Thread 2 is adding, 5
Thread 2 is adding, 6
Thread 2 is adding, 7
Thread 2 is adding, 8
Thread 2 is adding, 9
90

```
# what if something wrong happens to t1
```rust
use std::{
    ops::AddAssign,
    sync::Mutex,
    thread::{scope, spawn},
};

fn test_mutex() {
    let mut score: Mutex<u16> = Mutex::new(0u16);

    let myfunc = || {
        let mut data = score.lock().unwrap();
        panic!("Error in thread 1"); //Painic
        for i in 1..10 {
            data.add_assign(i);
            println!("Thread 1 is adding {}", i);
        }
    }; //mutex guard will get outof scope and returns contorl back to the caller.

    let myfunc2 = || {
        println!("T2, Wating for mutex lock");
        let mut data = score.lock().unwrap();
        for i in 1..10 {
            data.add_assign(i);
            println!("Thread 2 is adding, {}", i);
        }
    };

    _ = scope(|s| {
        s.spawn(myfunc);
        s.spawn(myfunc2);
    });
    println!("{:?}", score.lock().unwrap());
}

fn main() {
    test_mutex();
}
```
This will stop the whole execution.
because t2 will never get the lock because the mutex guard never drops.
# Handle the error

```rust

    ops::AddAssign,
    sync::Mutex,
    thread::{scope, spawn},
};

fn test_mutex() {
    let mut score: Mutex<u16> = Mutex::new(0u16);

    let myfunc = || {
        let mut data = score.lock().unwrap();


        drop(data); // Will drop the mutex guard
        panic!("Error in thread 1");



        for i in 1..10 {
            data.add_assign(i);
            println!("Thread 1 is adding {}", i);
        }
    }; //mutex guard will get outof scope and returns contorl back to the caller.

    let myfunc2 = || {
        println!("T2, Wating for mutex lock");
        let mut data = score.lock().unwrap();
        for i in 1..10 {
            data.add_assign(i);
            println!("Thread 2 is adding, {}", i);
        }
    };

    _ = scope(|s| {
        let handle1 = s.spawn(myfunc).join();
        let handle2 = s.spawn(myfunc2).join();
        if handle1.is_err() {
            println!("Thread 1 has an error")
        }
    });
    println!("{:?}", score.lock().unwrap());
}

fn main() {
    test_mutex();
}
```

```
thread '<unnamed>' panicked at src/main.rs:14:9:
Error in thread 1
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
T2, Wating for mutex lock
Thread 2 is adding, 1
Thread 2 is adding, 2
Thread 2 is adding, 3
Thread 2 is adding, 4
Thread 2 is adding, 5
Thread 2 is adding, 6
Thread 2 is adding, 7
Thread 2 is adding, 8
Thread 2 is adding, 9
Thread 1 has an error
45

```

# Message Passing with MPSC Channels (queue)


MPSC -> Multiple producer single consumer
```rust
use std::sync::mpsc;

fn test_channels() {
    let (tx, rx) = mpsc::channel::<u8>();

    let send_status1 = tx.send(100);
    drop(rx); //lets drop the rx so that the send fails.
    let send_status2 = tx.send(12);
    tx.send(1);

    println!("Send success: {}", send_status1.is_ok()); //true

    println!("Send2 success: {}", send_status2.is_ok()) //false
}

fn main() {
    test_channels();
}
```
Receiving
```rust
use std::sync::mpsc;
use std::time::Duration;

fn test_channels() {
    let (tx, rx) = mpsc::channel::<u8>();

    let send_status1 = tx.send(100);
    println!("Send success: {}", send_status1.is_ok()); //true

    //Receive
    let reveive_result = rx.recv_timeout(Duration::from_millis(300)); //300 ms is the max time to wait for a message
    println!(
        "Reveived: {} , Message: {}",
        reveive_result.is_ok(),
        reveive_result.unwrap()
    ); //True, 100
}

fn main() {
    test_channels();
}
```

