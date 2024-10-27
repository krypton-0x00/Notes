### Operators and Operator Overloading in C++
```cpp
//the og example
struct YoutubeChannel {
	string Name;
	int subs;
	YoutubeChannel(string name,int  subs){
		Name = name
		subs = subs
	}
	
};
ostream& operator << (ostream& COUT , YoutubeChannel& ytChannel){
	COUT<< ytChannel.Name<<std::endl;
	COUT<<ytChannel.Subs<<std::endl;
	return COUT;
}
//Now if we try to print the USER DEFINED TYPE i.e YoutubeChannel
YoutubeChannel yt1 = YoutubeChannel("BEB",123);
cout<<yt1; //BEB and 123

//there is a way to invoke also but above one is better;
operator<<(cout,yt1); //BEB and 123 ; same as above
```


## Lets overload += ;
```cpp
struct MyCollection{
	list<YoutubeChannel>mychannels;
}
int main(){
	MyCollection mycollection;
	mycollection += yt1; //lets implement this;
}
```


-------------------
```cpp
struct MyCollection{
	list<YoutubeChannel>mychannels;

	void operator+= (YoutubeChannel& channel){ //since we are in inside the struct the Mycollection will be accessable so we dont need to pass it;
		this->myChannel.push_back(channel); //this mychannel is the firstParam;
		
	}
}

int main(){
	MyCollection mycollection;
	mycollection += yt1; //lets implement this;
}
```
#### Operators

Operators are special symbols used to perform operations on variables and values. Examples of operators include:

- Arithmetic operators: `+`, `-`, `*`, `/`, `%`
- Comparison operators: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Logical operators: `&&`, `||`, `!`
- Assignment operators: `=`, `+=`, `-=`, etc.

#### Operator Overloading

Operator overloading allows you to redefine the behavior of operators for user-defined types (like classes or structs). This means that you can define how an operator behaves when applied to objects of your class. Not all operators can be overloaded, but most can.

For example, you can overload the `+` operator to add two objects of a custom class together, or overload the `<<` operator to print a custom object.

### How to Overload Operators

You can overload operators by defining a special member function or a non-member function. The format generally looks like this:

 

```cpp
return_type operator op (parameter_list) {  
	// implementation
}
```

Here, `op` is the operator you want to overload (like `+`, `*`, `<<`, etc.), and `parameter_list` is the arguments required for the operator function.

#### Rules for Operator Overloading:

1. **At least one operand must be a user-defined type** (such as a class or struct).
2. **Precedence and associativity of operators** cannot be changed.
3. You **cannot overload** certain operators like `::`, `sizeof`, `.` (dot operator), and `?:` (ternary operator).
# List of Overloadable Operators:

### 1. **Arithmetic Operators**

- `+` (Addition)
- `-` (Subtraction)
- `*` (Multiplication)
- `/` (Division)
- `%` (Modulus)

### 2. **Relational (Comparison) Operators**

- `==` (Equal to)
- `!=` (Not equal to)
- `<` (Less than)
- `>` (Greater than)
- `<=` (Less than or equal to)
- `>=` (Greater than or equal to)

### 3. **Logical Operators**

- `&&` (Logical AND)
- `||` (Logical OR)
- `!` (Logical NOT)

### 4. **Bitwise Operators**

- `&` (Bitwise AND)
- `|` (Bitwise OR)
- `^` (Bitwise XOR)
- `~` (Bitwise NOT)
- `<<` (Bitwise left shift)
- `>>` (Bitwise right shift)

### 5. **Assignment Operators**

- `=` (Assignment)
- `+=` (Add and assign)
- `-=` (Subtract and assign)
- `*=` (Multiply and assign)
- `/=` (Divide and assign)
- `%=` (Modulus and assign)
- `&=` (Bitwise AND and assign)
- `|=` (Bitwise OR and assign)
- `^=` (Bitwise XOR and assign)
- `<<=` (Left shift and assign)
- `>>=` (Right shift and assign)

### 6. **Unary Operators**

- `+` (Unary plus)
- `-` (Unary minus)
- `*` (Dereference)
- `&` (Address-of)
- `++` (Increment, both prefix and postfix)
- `--` (Decrement, both prefix and postfix)

### 7. **Subscript and Function Call Operators**

- `[]` (Subscript operator)
- `()` (Function call operator)

### 8. **Pointer Operators**

- `->` (Member access through pointer)
- `->*` (Pointer-to-member)

### 9. **Memory Management Operators**

- `new` (Allocate memory)
- `delete` (Deallocate memory)
- `new[]` (Allocate memory for array)
- `delete[]` (Deallocate memory for array)

### 10. **Other Operators**

- `,` (Comma operator)
- `==` (Equal to)
- `!=` (Not equal to)
- `<<` (Stream insertion, often used with `std::ostream`)
- `>>` (Stream extraction, often used with `std::istream`)
- `~` (Destructor)
- `()` (Function call)

# Operators that **Cannot** be Overloaded

- `.` (Member access)
- `.*` (Pointer-to-member access)
- `::` (Scope resolution)
- `?:` (Ternary conditional)
- `sizeof` (Size of operator)
- `typeid` (Type identification)
- `static_cast`, `dynamic_cast`, `const_cast`, and `reinterpret_cast` (Type casting)


# Example without operator overloading
```cpp
#include<iostream>
#include<vector>

using namespace std;

struct Vector2
{
    float x,y;
    Vector2(float x, float y):x(x),y(y){}

    Vector2 Add(const Vector2& other)const {
        return Vector2(x+other.x,y+other.y);
    }
    Vector2 Multiply(const Vector2& other)const {
        return Vector2(x*other.x,y*other.y);
    }
};


int main(){
    Vector2 position(0.2f,4.0f);
    Vector2 speed(0.2f,4.0f);
    Vector2 powerup(1.1f,1.1f);

    
    Vector2 result = position.Add(speed.Multiply(powerup));


}
```
# Using Overloading (the best syntax)
```cpp
#include<iostream>
#include<vector>

using namespace std;

struct Vector2
{
    float x,y;
    Vector2(float x, float y):x(x),y(y){}

    Vector2 Add(const Vector2& other)const {
        return Vector2(x+other.x,y+other.y);
    }
    Vector2 Multiply(const Vector2& other)const {
        return Vector2(x*other.x,y*other.y);
    }
//overloads for + and *;
    Vector2 operator+(const Vector2& other)const{
        return Add(other);
    }
    Vector2 operator*(const Vector2& other)const{
        return Multiply(other);
    }
};


int main(){
    Vector2 position(0.2f,4.0f);
    Vector2 speed(0.2f,4.0f);
    Vector2 powerup(1.1f,1.1f);


    Vector2 result = position + speed * powerup;


}

```

# OR a weird syntax (Ugly as f**k)
```cpp
#include<iostream>
#include<vector>

using namespace std;

struct Vector2
{
    float x,y;
    Vector2(float x, float y):x(x),y(y){}

    Vector2 Add(const Vector2& other)const {
    //this
       return *this + other;
    }
    Vector2 Multiply(const Vector2& other)const {
	//or this is same
	 return operator*(other);
       
    }
    Vector2 operator+(const Vector2& other)const{
        return Vector2(x*other.x,y*other.y);
    }
    Vector2 operator*(const Vector2& other)const{
        return Multiply(other);
    }
};


int main(){
    Vector2 position(0.2f,4.0f);
    Vector2 speed(0.2f,4.0f);
    Vector2 powerup(1.1f,1.1f);


    Vector2 result = position + speed * powerup;


}
```


# Example Implementation
```cpp
int main(){
    Vector2 position(0.2f,4.0f);
    Vector2 speed(0.2f,4.0f);
    Vector2 powerup(1.1f,1.1f);


    Vector2 result = position + speed * powerup;

    cout<<result<<endl;  //Error: NO operator match << these operands;


}
```
## FIX
```cpp

std::ostream& operator<<(std::ostream& stream,const Vector2& other){
    stream << other.x<<", "<<other.y;
    return stream;
}


int main(){
    Vector2 position(0.2f,4.0f);
    Vector2 speed(0.2f,4.0f);
    Vector2 powerup(1.1f,1.1f);


    Vector2 result = position + speed * powerup;

    cout<<result<<endl; //4.55, 5.00 //this is just a testing value not the real data;


}
```