# TypeScript Fundamentals

## 1. Types
TypeScript introduces a type system that provides type safety. The basic types in TypeScript include:
- **number**: Represents both integer and floating-point numbers.
- **string**: Represents text data.
- **boolean**: Represents true/false values.
- **any**: A type that can hold any value (use with caution).
- **void**: Represents the absence of a value, typically used in functions that do not return a value.
- **null** and **undefined**: Used to denote absence of values.

## 2. Interfaces
Interfaces in TypeScript can define contracts for classes or objects. An interface can describe the shape of an object.

```typescript
interface User {
    name: string;
    age: number;
}
```

## 3. Classes
TypeScript supports object-oriented programming with classes. Classes can have properties, methods, and constructors.

```typescript
class Animal {
    constructor(public name: string) {}
    move(distanceInMeters: number = 0) {
        console.log(`{this.name} moved ${distanceInMeters}m.`);
    }
}
```

## 4. Generics
Generics provide a way to create reusable components that can work with any data type.

```typescript
function identity<T>(arg: T): T {
    return arg;
}
```

## 5. Decorators
Decorators allow you to add metadata to classes and members.

```typescript
function Deprecated(message: string) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        console.warn(`${propertyKey} is deprecated: ${message}`);
    };
}
```

## 6. Configuration
TypeScript can be configured using a `tsconfig.json` file which allows customizing the compilation process.

Example configuration:
```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "strict": true
  }
}
```

## 7. Best Practices
- Use `strict` mode to catch potential issues.
- Prefer `interface` over `type` when defining object shapes.
- Consistently use proper type annotations.

## 8. Common Errors
- Failing to install type definitions (e.g., `@types/...`).
- Type mismatch errors when using third-party libraries.

## 9. Learning Resources
- [TypeScript Official Documentation](https://www.typescriptlang.org/docs/)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)

## 10. Practical Exercises
1. Create a class representing a car with properties like make, model, and methods to get car details.
2. Implement a simple calculator using generics to perform operations on numbers.
3. Use interfaces to define a user profile and create a function to validate user input.
