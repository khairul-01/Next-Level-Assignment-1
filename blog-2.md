# How *Generics* help build reusable and strictly typed components in TypeScript

## Introduction
In TypeScript, we often need to write reusable code that work with different types of data. Without Generics, we may either duplicate core or use unsafe type *any*, which removes type safety.
Generics solve this problem by allowing function, classes, interfaces, and components to work with multiple data types while still keeping strong type checking.
In simple words:- Generics allow us to create flexible yet strictly typed code.
Generics help TypeScript understand the relationship between input and output types, ensuring safety regardless of the data structure being used.

## What are Generics?
Generics are a feature in TypeScript that lets us create reusable code using type placeholders.
A generic type is usually written using angle bracket(<>).
Example:
```TypeScript
function identity<T>(value: T): T {
    return value;
}
```
Here:
- **T** is a generic type parameter
- It acts like a placeholder for any type
- TypeScript replaces **T** with the actual type when the function is called

## Why are Generics important?
Without Generics, we often use:
- Duplicate function
- Unsafe any type
Example without Generics:
```TypeScript
function stringFunction(value: sting): string {
    return value;
}

function numberFunction(value: number): number {
    return value;
}
```
This create repetitive code.
Using Generics:
```TypeScript
function identity<T>(value: T): T {
    return value;
}
```
Now the same function works for all types safely.

## How Generics maintain strict typing
Generics preserve the original type information.
Example:
```TypeScript
function identity<T>(value: T): T {
  return value;
}

const result = identity<string>("Hello");
```
TypeScript knows:- result is string
If we pass a number: 
```TypeScript
const result = identity<number>(100);
```
Now TypeScript automatically treats result as a number.
This strict typing prevents type-related bugs.

## Generic Functions
Generic functions are the most common use of Generics.
Example:
```TypeScript
function getFirstElement<T>(arr: T[]): T {
    return arr[0];
}
```
Usage:
```TypeScript
const firstNumber = getFirstElement([1, 2, 3]);
const firstString = getFirstElement(["A", "B", "C"]);
```
TypeScript automatically infers the types:
- firstNumber -> number
- firstSting -> string

## Generic Interfaces
Generics can also be used with interfaces.
Example:
```TypeScript
interface ApiResponse<T> {
    success: boolean;
    data: T;
}
```
Usage:
```TypeScript
interface User {
    name: string;
    age: number;
}

const response: ApiResponse<User> = {
    success: true;
    data: {
        name: "John",
        age: 25,
    }
}
```
Now *data* must follow the *User* structure.
This improves consistency.

## Generic Classes
Generic also work with classes
```TypeScript
class Box<T> {
  content: T;

  constructor(value: T) {
    this.content = value;
  }
}
```
Usage:
```TypeScript
const numberBox = new Box<number>(100);
const stringBox = new Box<string>("Hello");
```
Each object maintains its own type safely.

## Generics in Reusable Components
Generics are extremely useful in frontend frameworks like React.
Example:
```TypeScript
type ListProps<T> = {
  items: T[];
  renderItem: (item: T) => string;
};
```
This component can render:
- User lists
- Product lists
- Order lists
while still keeping accurate types.

## Generic Constraints
Sometimes we want Generics to allow only certain types.
This is called a constraint.
Example:
```TypeScript
function getLength<T extends { length: number }>(
  item: T
): number {
  return item.length;
}
```
Now only values with a *length* property are allowed.
Valid:
```TypeScript
getLength("Hello");
getLength([1, 2, 3]);
```
Invalid:
```TypeScript
getLength(100);
```
TypeScript prevents mistakes automatically.

## Conclusion
Generics are one of the most powerful features in TypeScript because they allow us to build reusable and flexible code without sacrificing type safety.
They help functions, classes, interfaces, and components adapt to different data structures while preserving strict typing. Unlike ***any***, Generics maintain accurate type information and protect applications from many runtime errors.
By using Generics properly, we can create scalable, maintainable, and reliable applications with less duplicate code and stronger type protection.