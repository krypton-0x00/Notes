```rust
#[allow(dead_code)]
async fn my_async_call(url: &str)->Result<serde_json::Value,reqwest::Error>{
    let response:serde_json::Value = reqwest::get(url)
    .await?
    .json::<serde_json::Value>()
    .await?;
    Ok(response)

}



#[cfg(test)]
mod tests {
    use super::*;
    #[tokio::test]
    async fn tests_calls_async_fn(){
        let api_url: &str = "https://jsonplaceholder.typicode.com/todos/1";
        let my_res:Result<serde_json::Value,reqwest::Error> = my_async_call(api_url).await;
        match my_res {
            Ok(r) =>{ dbg!(r);},
            Err(_) => {panic!("Error");}
        }
    }

}





fn main() {
     
}

```