```rust
use serde_json;
use serde::{Serialize,Deserialize};

#[derive(Serialize,Deserialize)]
struct Person{
    name:String,
    age: u8,
}
fn main() {
    let person = Person{name:"Shakir".to_string(),age:19};   

    //convert to json format (Serialize)
    let json = serde_json::to_string(&person).unwrap();

    println!("Serialized data: {} ",json);
    
    //convert json back to struct (Deserialize)
    let data: Person = serde_json::from_str(&json).unwrap();
    println!("Deserialized: {} is {} years old",data.name, data.age);

}:
```