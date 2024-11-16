In TypeScript, `declare` is used for ambient declarations—basically, it tells the TypeScript compiler about types, variables, or modules that may exist in the global scope but aren't explicitly defined in the TypeScript files. `namespace` is a feature for organizing code, often used in larger codebases to group related functions, types, or variables.

Here are examples of both:

### 1. `declare` Example
`declare` is often used when you're integrating with third-party JavaScript libraries or you want to define a global variable or type without its implementation in TypeScript.

#### Declaring a Global Variable
```typescript
// declare a global variable
declare const API_URL: string;

console.log(API_URL); // TypeScript recognizes `API_URL` as a string
```

#### Declaring a Module
When working with libraries that don’t have TypeScript definitions, you can declare a module to inform the compiler about it.

```typescript
// declare a module for a library with no TypeScript definitions
declare module 'some-js-library' {
  export function someFunction(): void;
}
```

### 2. `namespace` Example
A `namespace` is a way to encapsulate and organize code, grouping related items like classes, interfaces, and functions.

```typescript
namespace MyNamespace {
  export class MyClass {
    greet() {
      return "Hello from MyNamespace!";
    }
  }

  export function myFunction() {
    return "This is a function in MyNamespace.";
  }
}

// Using items from the namespace
const myClassInstance = new MyNamespace.MyClass();
console.log(myClassInstance.greet());

console.log(MyNamespace.myFunction());
```

### Nested Namespaces
You can also nest namespaces for more detailed organization.

```typescript
namespace OuterNamespace {
  export namespace InnerNamespace {
    export function innerFunction() {
      return "Hello from InnerNamespace!";
    }
  }
}

// Using the nested namespace
console.log(OuterNamespace.InnerNamespace.innerFunction());
```

### Using `declare` with `namespace`
Sometimes, you may need to declare a namespace for a global library.

```typescript
declare namespace MyGlobalLibrary {
  export function globalFunction(): string;
}

// Usage
console.log(MyGlobalLibrary.globalFunction());
```

These examples show how `declare` and `namespace` can be used to handle global variables or functions and to organize code effectively in TypeScript.