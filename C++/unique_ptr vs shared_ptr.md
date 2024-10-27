In C++, **smart pointers** like `std::unique_ptr` and `std::shared_ptr` are used to manage dynamic memory automatically, ensuring that memory is properly freed when it's no longer needed. They help prevent common issues like memory leaks and dangling pointers by automatically handling resource ownership and cleanup.
### 1. **`std::unique_ptr`**:
Scoped pointer->gets destroyed once it goes out of scope;
`std::unique_ptr` is a smart pointer that provides **exclusive ownership** of an object. This means that only one `unique_ptr` can own the object at a time. When the `unique_ptr` is destroyed or reset, the object is deleted.

#### Key Characteristics of `std::unique_ptr`:

- **Exclusive ownership**: Only one `unique_ptr` can point to a resource at any time.
- **Cannot be copied**: `std::unique_ptr` cannot be copied, only moved. This prevents multiple pointers from managing the same resource.
- **Efficient**: Since `std::unique_ptr` doesn't have the overhead of reference counting (as `std::shared_ptr` does), it is very lightweight.

EG:
```cpp
int main(){
	{
		std::unique_ptr<Enitity>entity(new Entity());
		entity->Print();
		//Preffered Way
		std::unique_ptr<Enitity>entity = std::make_unique<Enitity>(); //prevents dangling pointers;
		entity->Print();
	}
	//Enitity Gets destroyed here;

}
```
#### Example:
```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass() {
        std::cout << "MyClass Constructor" << std::endl;
    }
    ~MyClass() {
        std::cout << "MyClass Destructor" << std::endl;
    }
};

int main() {
    // Create a unique_ptr to manage MyClass
    std::unique_ptr<MyClass> ptr1 = std::make_unique<MyClass>();

    // ptr1 manages the MyClass object
    // std::unique_ptr cannot be copied
    // std::unique_ptr<MyClass> ptr2 = ptr1; // Error

    // std::unique_ptr can be moved
    std::unique_ptr<MyClass> ptr2 = std::move(ptr1);

    // Now ptr2 owns the MyClass object, and ptr1 is empty (nullptr)
    return 0;
}

```

#### Key Operations for `std::unique_ptr`:

- **Move semantics**: You can transfer ownership using `std::move`, but after that, the original `unique_ptr` becomes null.
- **Custom deleters**: You can provide a custom deleter for cleaning up resources.

### 2. **`std::shared_ptr`**:

`std::shared_ptr` is a smart pointer that allows **shared ownership** of an object. Multiple `shared_ptr` instances can manage the same object, and the object is destroyed when the last `shared_ptr` that owns it is destroyed or reset.

#### Key Characteristics of `std::shared_ptr`:

- **Shared ownership**: Multiple `shared_ptr` instances can own the same resource. The resource is deleted only when the last `shared_ptr` is destroyed.
- **Reference counting**: `std::shared_ptr` maintains a reference count, which keeps track of how many `shared_ptr` instances own the object. When the count reaches zero, the object is deleted.
- **Thread-safe**: The reference count is thread-safe, but the object itself is not. If multiple threads are accessing the object, you need additional synchronization.

#### Example:
```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass() {
        std::cout << "MyClass Constructor" << std::endl;
    }
    ~MyClass() {
        std::cout << "MyClass Destructor" << std::endl;
    }
};

int main() {
    // Create a shared_ptr to manage MyClass
    std::shared_ptr<MyClass> ptr1 = std::make_shared<MyClass>();

    // Another shared_ptr pointing to the same object
    std::shared_ptr<MyClass> ptr2 = ptr1;

    std::cout << "Use count: " << ptr1.use_count() << std::endl; // Output: 2

    // When ptr1 and ptr2 go out of scope, the MyClass object is deleted
    return 0;
}

```

#### Key Operations for `std::shared_ptr`:

- **Reference counting**: Multiple `shared_ptr` instances can share the same object, and the object is deleted when the last owner releases it.
- **`use_count()`**: You can check how many `shared_ptr` instances share ownership of the object.
- **`weak_ptr`**: Use `std::weak_ptr` to break cyclic references that can cause memory leaks when using `std::shared_ptr`.
### Differences Between `std::unique_ptr` and `std::shared_ptr`:

|Feature|`std::unique_ptr`|`std::shared_ptr`|
|---|---|---|
|**Ownership**|Exclusive (only one pointer can own the object)|Shared (multiple pointers can own the object)|
|**Copying**|Cannot be copied, only moved|Can be copied and shared|
|**Reference Counting**|No|Yes (uses reference counting)|
|**Overhead**|Low (no reference counting)|Higher (due to reference counting)|
|**Deletion**|Object is destroyed when `unique_ptr` is destroyed or reset|Object is destroyed when the last `shared_ptr` is destroyed or reset|
|**Use Case**|When only one owner is needed|When multiple owners are needed|
# Some Pointer Types
|Pointer Type|Description|
|---|---|
|**Null Pointer**|Points to nothing (`nullptr`).|
|**Void Pointer**|Can point to any data type. Needs casting to dereference.|
|**Wild Pointer**|Uninitialized pointer pointing to a random memory location.|
|**Dangling Pointer**|Points to a memory location that has been freed or deleted.|
|**Smart Pointer**|Automatically manages memory (e.g., `unique_ptr`, `shared_ptr`).|
|**Function Pointer**|Points to a function.|
|**Null-terminated Pointer**|Points to a C-string (character array ending with `'\0'`).|
|**Pointer to Pointer**|Points to another pointer.|
|**Constant Pointer**|The pointer or the value it points to, or both, cannot change.|
