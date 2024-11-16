
# .chain() and collect()
```rust
fn main() {
    let mut fruits: Vec<&str> = vec!["apple","mango","banana"];
    let mut veg : Vec<&str> = vec!["carrot","radish","turnip"];
    
    let mut allItems = fruits.iter().chain(&veg);
    
    for item in &allItems {
        println!("Eating {}", item);
    }
    
    
}

//output 
Eating apple
Eating mango
Eating banana
Eating carrot
Eating radish
Eating turnip
```


```rust

fn main() {
    let mut fruits: Vec<&str> = vec!["apple","mango","banana"];
    let mut veg : Vec<&str> = vec!["carrot","radish","turnip"];
    
    let mut allItems = fruits.iter().chain(&veg);
    
    let mut backToCollection: Vec<&&str> = allItems.collect();
    
    println!("{:?}",backToCollection);
    
}
 
//OUTPUT
// ["apple", "mango", "banana", "carrot", "radish", "turnip"]
```


Here’s why that happens:

1. **`fruits` and `veg` contain `&str` elements**, which are already references to string literals.
2. **Calling `iter()` on these vectors returns an iterator over `&str` references** to each element (so each item yielded by `iter()` is of type `&&str`).
3. **The `collect()` method's type** will match the iterator's item type, which is `&&str` in this case.



### Lets convert the &str to heap allocated str
```rust
```
"apple fruit"
"mango fruit"
"banana fruit"
```rust
fn main() {
    let mut fruits: Vec<&str> = vec!["apple","mango","banana"];
//convert &str to String
    let mut heap_fruits = fruits.iter().map(|e|String::from(*e));
    
    println!("{:?}",heap_fruits);
    
    //add fruit to the end of each item.
    let mut new_heap_fruits = heap_fruits.map(|mut e:String| {e.push_str(" fruit"); return e;});
  
    new_heap_fruits.for_each(|e|println!("{:?}",e));

//GET LAST ELEMENT 

new_heap_fruits.last(); // "banana"
  
}
 
//OUTPUT
"apple fruit"
"mango fruit"
"banana fruit"
```