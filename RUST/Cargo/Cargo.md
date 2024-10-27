Cargo has 2 profiles 
1. Dev (unopt)
2. Release (opt)
By default if we run `cargo build` it will use dev profile
To compile with the release we can run :
`cargo build --release`


We can change the optimization level of each profiles in` Cargo.toml`
```toml
//default optimization
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

Levels
1. No optimization 
3. The highest No of optimazations
