In TypeScript, you can merge namespaces in several ways, allowing you to split definitions across multiple files or augment existing types. This is particularly useful in large applications or when extending functionality.

### 1. Merging Namespaces in the Same File

If you define the same namespace multiple times, TypeScript will automatically merge them. Here's an example of how it works:

```typescript
namespace Library {
  export function functionOne() {
    return "Function One";
  }
}

// Add more to the same namespace
namespace Library {
  export function functionTwo() {
    return "Function Two";
  }
}

// Using the merged namespace
console.log(Library.functionOne()); // Output: "Function One"
console.log(Library.functionTwo()); // Output: "Function Two"
```

In this example, both `functionOne` and `functionTwo` are part of the `Library` namespace, even though they were defined separately.

### 2. Merging Namespaces Across Multiple Files

When working in a multi-file setup, TypeScript can merge namespaces across different files if they are in the same project or included in the same compilation step. 

#### File 1: `library-one.ts`
```typescript
namespace Library {
  export function functionOne() {
    return "Function One";
  }
}
```

#### File 2: `library-two.ts`
```typescript
namespace Library {
  export function functionTwo() {
    return "Function Two";
  }
}
```

When you compile both files, TypeScript will merge the contents of `Library` from both files, allowing you to use both `functionOne` and `functionTwo` as part of the `Library` namespace.

### 3. Merging Namespaces with Classes, Interfaces, or Enums

You can also merge namespaces with other TypeScript structures like classes, interfaces, and enums. This technique is commonly used for augmenting types.

#### Merging a Namespace with a Class
You can add static members to a class using a namespace with the same name:

```typescript
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}

// Merging a namespace to add static members to `Animal`
namespace Animal {
  export function create(name: string): Animal {
    return new Animal(name);
  }
}

// Using the merged class and namespace
const myAnimal = Animal.create("Lion");
console.log(myAnimal.name); // Output: "Lion"
```

#### Merging a Namespace with an Interface
You can extend an interface by merging it with a namespace:

```typescript
interface Person {
  name: string;
}

// Merging a namespace with the `Person` interface to add related functionality
namespace Person {
  export function greet(person: Person) {
    return `Hello, ${person.name}`;
  }
}

// Usage
const person: Person = { name: "Alice" };
console.log(Person.greet(person)); // Output: "Hello, Alice"
```

### 4. Merging External Modules with Namespaces

When working with modules, you can also merge namespaces to add functionality. This is useful for augmenting a third-party library's types.

```typescript
// Third-party library augmentation
import * as moment from 'moment';

declare module 'moment' {
  namespace moment {
    export function customFunction(): string;
  }
}

// Implementing the custom function
moment.customFunction = () => "Custom Function";

console.log(moment.customFunction()); // Output: "Custom Function"
```

### Summary

Merging namespaces allows you to expand functionality in an organized way by:
- Defining namespaces in different parts of your code.
- Augmenting classes, interfaces, or third-party libraries with additional methods or properties.
