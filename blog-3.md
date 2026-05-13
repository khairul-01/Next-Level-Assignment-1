# How *Pick* and *Omit* utility types prevent code duplication in TypeScript

## Introduction
In TypeScript, we often work with large interfaces that describe complex data structure. However, not every part of an application needs all the properties from a large interface.  
For example:  
- A registration form may only need a username and email
- A public profile may hide password
- An admin panel may require additional fields  
If we create separate interfaces manually for every situation, code duplication becomes a serious problem.  
To solve this, TypeScript provides two utility types:
- **Pick**
- **Omit**
These utility types help us create specialized "slice" of an existing interface while keeping the code DRY (Don't Repeat Yourself).
-----
## What does DRY mean?
DRY stands for: 
| Don't Repeat Yourself   
This principle means:
- Avoid writing the same code repeatedly
- Reuse existing structures whenever possible
- Keep code centralized
- Reduce maintenance effort
A DRY codebase is:
- Cleaner
- Easier to maintain
- Less error-prone
- More scalable         

## The problem without *Pick* and *Omit*
Suppose we have a master interface:
```TypeScript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
    role: string;
}
```
Now imagine we need:
1. A public profile
2. A login form
3. An admin view
Without utility types, we might write:
```ts
interface PublicUser {
    id: number;
    name: string;
    email: string;
}

interface LoginUser {
    email: string;
    password: string;
}
```
This duplicates properties repeatedly.
-----
## Why code duplication is bad
Duplicated code creates maintenance problems.  
Imagine updating the email type:
```ts
email: string | null;
```
Now every duplicated interface must also be updated manually.  
This increases:
- Bugs
- Inconsistency
- Development time  

## What is *Pick*?
Pick creates a new type by selecting specific properties from an existing interface.  
Syntax:
```ts
Pick<Type, Key> 
```
## Example of *Pick*
Master interface:
```ts
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
    role: string;
}
```
Create a public user type:
```ts
type PublicUser = Pick<User, "id" | "name" | "email">;
```
Result:
```ts
{
    id: number;
    name: sting;
    email: string;
}
```
TypeScript automatically extracts those fields.

## What is *Omit*?
*Omit* does the opposite of *Pick*.
Instead of selecting properties, it removes properties from a type.  
Syntax:  
```ts
Omit<Type, Keys>
```

### Example of *Omit*
Master interface:  
```ts
interface User {
    id: number;
    name: string;
    email: sting;
    password: string;
    role: string;
}
```
Remove sensitive information:  
```ts
type SafeUser = Omit<User, "password">;
```
Result:
```ts
{
    id: number;
    name: string;
    email: string;
    role: string;
}
```
### Why *Omit* is useful
*Omit* is helpful when:
- Hiding private fields
- Creating public API responses
- Removing unnecessary data
- Simplifying interfaces

## Example: Backend API Response
Database model:
```ts
interface DatabaseUser {
    id: number;
    name: string;
    email: string;
    password: string;
    secretKey: string;
}
```
Public API Response:
```ts
type PublicUser = Omit<DatabaseUser, "password" | "secretKey">;
```
This prevents accidental exposer of secure data.
-----
## Combining *Pick* and *Omit*
Sometimes developers combine both.  
Example:
```ts
type EditableUser = Pick<Omit<User, "id">, "name" | "email">;
```
This creates a specialized editable slice.
----
## How *Pick* and *Omit* keep code DRY
#### Centralized type definitions
We define properties once:
```ts
interface User {
    id: number;
    name: string;
    email: string;
}
```
Then reuse them everywhere.
---
#### Automatic Synchronization
If the master interface changes:
```ts
email: string | null;
```
All derived types update automatically.  
This reduces inconsistency.
---
#### Less Repeated Code
Without utility types:
```ts
interface UserPreview {
    id: number;
    name: string;
}
```
With utility types:
```ts
type UserPreview = Pick<User, "id" | "name">;
```
Its's much shorter and cleaner.
----
## Common use cases
***Pick***  
Used for:
- Forms
- UI cards
- API requests
- Partial views
- Lightweight components

***Omit***  
Used for:
- Hiding passwords
- Removing internal fields
- Sanitizing data
- Public API response

#### Combine with other utility types
Utility types work well with:
- *Partial*
- *Readonly*
- *Required*

## Conclusion
***Pick*** and ***Omit*** are powerful TypeScript utility that help us create specialized 'slice' of a master interface without duplicating code.
- ***Pick*** selects only the required properties
- ***Omit*** removes unnecessary properties
Together, they help maintain DRY principles by centralizing type definitions,reducing repetition, and automatically synchronizing changes across the codebase.  
By using these utility types properly, we can write cleaner, safer, more maintainable, and scalable TypeScript applications.