-> Frontend
-> Backend
-> Common

1. create a dir. blog 
2. create Cargo.toml
3. Add the following section 
```toml
[workspace]
members = [
    "blog_api","blog_web","blog_shared"
]

```
4. Create these packages
5. `cargo new --vcs none blog_api ` vcs none will not create a git repo
6. `cargo new --vcs none blog_web
7. `cargo new --vcs none --lib blog_shared
8. `cargo build` from the root of the project.
9. Now you will see there will be no separate target dir for each. there will be only one target dir. 
10. we can build a single/particular package using `cargo build -p blog_api`