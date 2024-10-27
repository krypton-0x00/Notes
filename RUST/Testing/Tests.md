```rust
#[cfg(test)]
mod tests {
#[test]
fn it_works() {
	assert_eq!(2 + 2, 4);
 }
}
```
For now, let’s ignore the top two lines and focus on the function to see how it works. Note the #[test] annotation : this attribute indicates this is a test function, so the test runner knows to treat this function as a test.

The function body uses the assert_eq! macro  to assert that 2 + 2 equals 4. This assertion serves as an example of the format for a typical test.
`cargo test`
Runs the test.


# Panics
Let’s add another test, but this time we’ll make a test that fails! Tests
fail when something in the test function panics. Each test is run in a new
thread, and when the main thread sees that a test thread has died, the test
is marked as failed.
```rust
#[cfg(test)]
mod tests {
 #[test]
 fn exploration() {
 assert_eq!(2 + 2, 4);
 }
 #[test]
 fn another() {
 panic!("Make this test fail");
 }
}
```
`cargo test`
```rust
running 2 tests
test tests::exploration ... ok
 test tests::another ... FAILED
 failures:
---- tests::another stdout ----
 thread 'tests::another' panicked at 'Make this test fail', src/lib.rs:10:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.
 failures:
 tests::another
 test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
error: test failed
```
# AssertEQ example
```rust
#[derive(Debug)]
pub struct Rectangle {
 length: u32,
 width: u32,
}
impl Rectangle {
 pub fn can_hold(&self, other: &Rectangle) -> bool {
 self.length > other.length && self.width > other.width
 }
}

#[cfg(test)]
mod tests {
use super::*; //anything we define in the outer module is available to this tests module
 #[test]
fn larger_can_hold_smaller() {
	 let larger = Rectangle { length: 8, width: 7 };
	 let smaller = Rectangle { length: 5, width: 1 };
	 assert!(larger.can_hold(&smaller));
 }
}

```
A test for can_hold that checks whether a larger rectangle can indeed hold a
smaller rectangle

# Testing Equality with the assert_eq! and assert_ne! Macros
. You could do this using the assert! macro and passing it an expression
using the == operator. However, this is such a common test that the standard
library provides a pair of macros—assert_eq! and assert_ne!—to perform this
test more conveniently
These macros compare two arguments for equality or inequality, respectively. They’ll also print the two values if the assertion fails, which makes it easier to see why the test failed
```rust 
pub fn add_two(a: i32) -> i32 {
 a + 2
 }
#[cfg(test)]
mod tests {
 use super::*;
 #[test]
 fn it_adds_two() {
 assert_eq!(4, add_two(2));
 }
}
```
# Adding Custom Failure Messages
You can also add a custom message to be printed with the failure message as optional arguments to the assert!, assert_eq!, and assert_ne! macros.
Any arguments specified after the one required argument to assert! or the two required arguments to assert_eq! and assert_ne! are passed along to the format! macro (discussed in “Concatenation with the + Operator or the format! Macro” on page 137), so you can pass a format string that contains {} placeholders and values to go in those placeholders.

```rust
pub fn greeting(name: &str) -> String {
	 format!("Hello {}!", name)
}
#[cfg(test)]
mod tests {
	 use super::*;
	 #[test]
	 fn greeting_contains_name() {
	 let result = greeting("Carol");
	 assert!(result.contains("Carol"),"Greeting did not contain name, value was `{}`", result);
	 }
}
```

if Fails
```
---- tests::greeting_contains_name stdout ----
 thread 'tests::greeting_contains_name' panicked at 'Greeting did not
contain name, value was `Hello!`', src/lib.rs:12:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```
# Checking for Panics with should_panic
In addition to checking that our code returns the correct values we expect, it’s also important to check that our code handles error conditions as we expect

```rust
pub struct Guess {
  value: u32,
}

impl Guess {
 pub fn new(value: u32) -> Guess {
	 if value < 1 || value > 100 {
		 panic!("Guess value must be between 1 and 100, got {}.", value);
	 }
	 Guess {
		 value
	 }
	 }
}


#[cfg(test)]
mod tests {
 use super::*;
 #[test]
 #[should_panic]
 fn greater_than_100() {
	 Guess::new(200);
 }
}
```
Testing that a condition will cause a panic!
We place the #[should_panic] attribute after the #[test] attribute and before
the test function it applies to. Let’s look at the result when this test passes:
```
running 1 test
test tests::greater_than_100 ... ok
test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```
lets add a bug
```rust
if value < 1{
 panic("Guess value must be bewteen 1 and 100 , got {}.",value)
}
```
```
running 1 test
test tests::greater_than_100 ... FAILED
failures:
failures:
 tests::greater_than_100
test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```


EXample 2
```rust
fn divide(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("division by zero");
    }
    a / b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn test_divide_by_zero() {
        divide(10, 0); // This should panic
    }

    #[test]
    fn test_normal_division() {
        assert_eq!(divide(10, 2), 5);
    }
}

```
## How it Works:
- In the test_divide_by_zero test, the function divide(10, 0) panics, and because of the #[should_panic] attribute, the test passes.
- If the function does not panic, the test would fail.
## Customizing #[should_panic] with an Expected Message

```rust
#[test]
#[should_panic(expected = "division by zero")]
fn test_divide_by_zero_with_message() {
    divide(10, 0); // Panics with "division by zero"
}

```
# Notes:
- Panic occurs: If the function panics as expected, the test passes.
- No panic occurs: If the function does not panic, the test fails.
- Wrong panic message: If you specify expected and the panic message does not match, the test fails.
# Integration Tests
We create a tests directory at the top level of our project directory, next to
src. Cargo knows to look for integration test files in this directory. We can
then make as many test files as we want to in this directory, and Cargo will
compile each of the files as an individual crate.

Let’s create an integration test. With the code in Listing 11-12 still in the
src/lib.rs file, make a tests directory, create a new file named tests/integration
_test.rs, and enter the code in Listing 11-13.

```rust
extern crate adder;
#[test]
fn it_adds_two() {
 assert_eq!(4, adder::add_two(2));
}
```
We’ve added extern crate adder at the top of the code, which we didn’t need in the unit tests. The reason is that each test in the tests directory is a separate crate, so we need to import our library into each of them
We don’t need to annotate any code in tests/integration_test.rs with #[cfg(test)]. Cargo treats the tests directory specially and compiles files in this directory only when we run cargo test. Run cargo test now:
