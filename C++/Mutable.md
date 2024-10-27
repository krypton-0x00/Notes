## 1. Use case 1
Allows functions , methods which are const to modify the variable which are marked as mutable


```cpp
using namespace std;

class MyClass {
    mutable int value;
public:
    MyClass(int v) : value(v) {}

    int getValue() const {  // const member function
	value = 12; // i am able to change the value even inside the const function;
        return value;
    }
};

int main() {
    MyClass obj(10);
    cout << "Value: " << obj.getValue() << endl;  // OK: getValue is const
    return 0;
}
```
## 2. With Lambdas
 