Virtual functions in C++ are a key feature of **polymorphism**, one of the pillars of Object-Oriented Programming (OOP). They allow derived classes to override methods from a base class, and they enable dynamic dispatch, where the method that gets called is determined at runtime rather than compile-time.

### What is a Virtual Function?

A **virtual function** is a function declared in the base class using the `virtual` keyword, and it is meant to be overridden in the derived class. When a function is marked as `virtual`, C++ ensures that the correct function is called for an object, regardless of the type of reference or pointer used to call it.

### How It Works

1. **Base class defines a virtual function**: The base class declares a function as `virtual`, signaling that this function can be overridden in any derived class.
2. **Derived class overrides the virtual function**: The derived class can then implement (or override) the virtual function.
3. **Virtual function tables (vtable)**: Behind the scenes, C++ uses a virtual table (`vtable`), which contains pointers to the actual functions that should be called. At runtime, based on the actual type of the object, C++ decides which function to call.


### Why Use Virtual Functions?

Virtual functions are used when you want to achieve **runtime polymorphism**, meaning that the correct function is selected based on the actual object type, not the reference or pointer type.

# without virtual Functions
```cpp
 
 
#include <iostream>

class Entity {
public:
    void getName() {
        std::cout << "Entity" << std::endl;
    }
};

class Player : public Entity{
public:
    void getName() {
        std::cout << "Player" << std::endl;
    }

};

int main()
{
    Entity* e = new Entity();
    e->getName(); //Enity
    Player* p = new Player();
    p->getName(); //

    Entity* p2 = p;
    p2->getName(); //Expect: Player but we get :: Entity
}
 

```

# Same but other example

```cpp
 
#include <iostream>

class Entity {
public:
    std::string getName() {
        return "Entity";
         
    }
};

class Player : public Entity{
public:
    std::string getName() {
        return "Player";
        
    }

};

void PrintName(Entity* entity) {
        
        std::cout << entity->getName() << std::endl;
}




int main()
{
    Entity* e = new Entity();
    PrintName(e); //Entity
    Player* p = new Player();
    PrintName(p); //Entity //but its Player right

    Entity* p2 = p;
    PrintName(p2); //Entity //this on is also player right
}
```

# With Virtual Functions
```cpp

 
#include <iostream>

class Entity {
public:
    virtual std::string getName() {
        return "Entity";
         
    }
};

class Player : public Entity{
public:
    virtual std::string getName() override {
         return "Player";
        
    }

};

void PrintName(Entity* entity) {
        
        std::cout << entity->getName() << std::endl;
}




int main()
{
    Entity* e = new Entity();
    PrintName(e); //Entity
    Player* p = new Player();
    PrintName(p); //Player 

    Entity* p2 = p;
    PrintName(p2); //Player  
}


```


 
# CONS (un noticeable performance issue  )
1. Requires more memory to store that v table so we can dispatch to the correct function that includes a member pointer in the base class that points to the v table
2. Secondly we have to go through   that table to look for  which function to map to .