```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

//create a waitGroup
var wg = sync.WaitGroup{} 

var dbData = []string{"id1","id2","id3","id4","id5"}

func main(){
	t0 := time.Now()
	for i:=0; i<4; i++{
		wg.Add(1)
		go dbCall(i)
	}
	wg.Wait()
	fmt.Println("Time taken is ", time.Since(t0) )
 	
}


func dbCall(i int){
	var dealy float32 = rand.Float32() * 2000
	time.Sleep(time.Duration(dealy) * time.Millisecond)
	fmt.Println("The value of i is ",dbData[i] )
	wg.Done()
}
```

Great! Let's start with **WaitGroups** and then explore **channels** for managing and synchronizing goroutines.

### WaitGroups
The `sync.WaitGroup` is used to wait for a set of goroutines to complete. It has three main methods:

- **`Add(n int)`**: increments the counter by `n`.
- **`Done()`**: decrements the counter by one.
- **`Wait()`**: blocks until the counter becomes zero.

#### Example Using WaitGroups

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done() // Marks this goroutine as done when it completes
    fmt.Printf("Worker %d starting\n", id)
    time.Sleep(time.Second) // Simulate some work
    fmt.Printf("Worker %d done\n", id)
}

func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 3; i++ {
        wg.Add(1) // Increment the WaitGroup counter
        go worker(i, &wg) // Start each worker in a separate goroutine
    }

    wg.Wait() // Wait for all workers to finish
    fmt.Println("All workers done")
}
```

### Explanation
1. **`wg.Add(1)`**: Adds 1 to the counter for each worker.
2. **`defer wg.Done()`**: Signals that a worker is done when it finishes.
3. **`wg.Wait()`**: Blocks the main function until the counter is zero, meaning all workers have finished.

### Channels
Channels provide a way to communicate between goroutines. They allow you to send and receive values, making synchronization easier.

- **Creating a Channel**: `ch := make(chan int)`
- **Sending a Value**: `ch <- value`
- **Receiving a Value**: `value := <-ch`

#### Example Using Channels

```go
package main

import (
    "fmt"
    "time"
)

func worker(id int, ch chan string) {
    time.Sleep(time.Second) // Simulate some work
    ch <- fmt.Sprintf("Worker %d done", id) // Send message to channel
}

func main() {
    ch := make(chan string)

    for i := 1; i <= 3; i++ {
        go worker(i, ch) // Each worker sends a message when done
    }

    for i := 1; i <= 3; i++ {
        message := <-ch // Receive message from channel
        fmt.Println(message)
    }
}
```

### Explanation
1. **Channel Creation**: `ch := make(chan string)` creates a channel for string messages.
2. **Worker Sends Message**: Each worker sends a completion message to the channel.
3. **Receiving Messages**: The main function receives and prints each message, blocking until each message arrives.

Would you like to explore buffered channels or learn about select statements next?
In Go, a **Mutex** (short for "mutual exclusion") is used to protect shared data from being accessed by multiple goroutines simultaneously, which could lead to data races or inconsistent results. It ensures that only one goroutine can access a critical section of code at a time.

The `sync` package provides a `Mutex` with two main methods:

- **`Lock()`**: Acquires the lock. If another goroutine has already locked it, the current goroutine will block until the lock is available.
- **`Unlock()`**: Releases the lock, allowing other goroutines to acquire it.

### Basic Example Using Mutex

Here's an example where we use a `Mutex` to control access to a shared variable.

```go
package main

import (
    "fmt"
    "sync"
)

type Counter struct {
    value int
    mux   sync.Mutex
}

func (c *Counter) Increment() {
    c.mux.Lock() // Acquire the lock before modifying the value
    c.value++
    c.mux.Unlock() // Release the lock after modification
}

func main() {
    var wg sync.WaitGroup
    counter := Counter{value: 0}

    for i := 0; i < 5; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for j := 0; j < 1000; j++ {
                counter.Increment()
            }
        }()
    }

    wg.Wait()
    fmt.Println("Final Counter Value:", counter.value)
}
```

### Explanation
1. **Counter Struct**: We define a `Counter` struct with a `value` field to keep track of the count and a `Mutex` to control access.
2. **`Increment()` Method**: Locks the `Mutex` before modifying `value` and unlocks it afterward to ensure safe concurrent access.
3. **Goroutines and WaitGroup**: We start multiple goroutines, each incrementing the counter 1000 times. `wg.Wait()` ensures the main function waits for all increments to complete.
4. **Mutex Control**: Without `mux.Lock()` and `mux.Unlock()`, the goroutines would access `counter.value` simultaneously, leading to inconsistent results.

### When to Use Mutex
Use a `Mutex` when:
- You need to protect shared data.
- You want to prevent simultaneous access by multiple goroutines.
