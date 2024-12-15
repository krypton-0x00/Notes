```rust
#[test]
fn create_and_get_resource_immutably() {
    let mut world = World::new();

    world.add_resource(FpsResource(60));
    if let Some(fps) = world.get_resource::<FpsResource>() {
        assert_eq!(**fps, 60)
    }
}
struct FpsResource(pub u32);

impl std::ops::Deref for FpsResource {
    type Target = u32;
    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```