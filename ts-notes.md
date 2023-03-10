# <img width=30 src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Typescript_logo_2020.svg/1200px-Typescript_logo_2020.svg.png" /> Notes
- Written by referring to <a href="https://www.linkedin.com/learning/learning-typescript-2?u=215525050">Learning TypeScript by Jess Chadwick </a> from LinkedIn Learning.

### Basic Types in JavaScript
- Boolean
- String
- Number
- BigInt
- Null
- Undefined
- Symbol
- Objects

### Types can be defined in two ways
1. using colon
```ts
    let <varname>:<type> ;
```
2. using `as` keyword
```ts
    let <varname> = <value> as <type>;
```

### Gradual Typing
It is a way to opt-in/out of the type checking. This can be useful when data is provided from third-party source or need to use the variable with existing codebase for compatibility purposes with JS.
This is achieved by using `any` type.
```ts
    let x: any = 123;
    x = "aum"
```
`Notes: One should always try to avoid 'any' type for maintaining type-safety.`

### Multiple Types
One can assign multiple types to a variable using pipe `|` (union) symbol without using `any` type like this:
```ts
let personID: string | number;
```
To make reusable multiple types, one can use the `type` keyword like this:
```ts
type idType = string | number;
let myID: idType;
```

### TypeScript configuration using ts-config.json
- Done for configuring the TS linter and TS compiler for usage.
- To be created in the root dir of the project.
```json
{
    "compilerOptions" : {
        "target": "es5",    // Version of JS to be compiled to.
        "outDir" : "dist",  // Compiled file directory.
        "allowJs": true,    // Inspect JS files in the project. Allowing compatibility with JS.
        "checkJs": true     // Check JS files for error.
    }
}
```

`Note : Ideally, we convert all JS files to TS files but we can interchangeablly use both of them.`

### Custom Types
#### Method 1: Returning Objects
One can return objects by giving their inline definition in function definition like this:
```ts
function person(id: number): {
    personName: string;
    personAge: number;
} {
    return {
        personName: "Aum",
        personAge: 22
    }
}
```

#### Method 2: Using interfaces
One can create reusable interfaces for creating custom types using `interface` contruct.
```ts
interface person {
    personName: string;
    personAge: number;
};
```

### Interfaces: A powerful tool.
Interfaces are used for creating custom types and are very powerful.
> #### 1. __Duck typing objects__ : Since JS supports this feature, if an object satisfies the strucure of an interface, then the object can be assigned to variables/passed as argument to that interface type.
```ts
    function setPerson(personDetails: person): void {
        ...
    }

    setPerson({
        personName: "Aum",
        personAge: 22
    });
```

> #### 2. __Methods__: We can also define methods in interfaces like this:
```ts
    interface person {
        personName: string;
        personAge: number;
        aboutPerson: (about: string) => string;
    };

    // or 

    interface person {
        personName: string;
        personAge: number;
        aboutPerson(about: string): string;
    };
```

> #### 3. __Ommiting properties in custom types__:
Tells TS that interface objects may not always have a particular property.<br>
Done by adding `?` after property name in interface definition.
```ts
interface person {
    personName: string;
    personAge: number;
    personEmail?: string
    aboutPerson?: (about: string) => string;
};

// A person may not have an email id.

let person1 = {
    personName: "aum",
    personAge: 22, 
    aboutPerson()
};
```

> #### 4. __ReadOnly properties__:
Properties can be defined as read-only by using the key word `readonly`.
```ts
    interface person {
        readonly personID: string;
        personName: string;
        personAge: number;
        personEmail?: string;
        personProfession: string
        aboutPerson?: (about: string) => string;
    };
```

### Restricting values:
Values can be restricted by using two methods:
> [1] __enums__: A strongly-typed object which that defines a set of named values.
```ts
        enum Profession {
            Student,
            Employee
        };

        interface person {
            readonly personID: string;
            personName: string;
            personAge: number;
            personEmail?: string;
            personProfession: Profession
            aboutPerson?: (about: string) => string;
        };

        let person1 = {
            personName: "aum",
            personAge: 22, 
            personProfession: Profession.Student;
            aboutPerson()
        };
    ```
    The above code results in the following JS:
    ```js
        var Profession;
        (function(PersonProfession) {
            Profession[Profession["Student"] = 0] = "student";
            Profession[Profession["Employee"] = 1] = "employee";
        })();
```

    `enum` by-default assigns numerical values to all the `enum-values` starting from 0. To alter this behaviour, custom values can be given as follows:

```ts
        enum Profession {
            Student = "Student";
            Employee = "Employee";
        };
```
    The will result in the following JS:
```js
        var Profession;
        (function(PersonProfession) {
            Profession["Student"] = "student";
            Profession["Employee"] = "employee";
        })();
```

> [2] __Literal types__ : Seperating restricted values with pipe `|` symbol.
```ts
        interface person {
            readonly personID: string;
            personName: string;
            personAge: number;
            personEmail?: string;
            personProfession: "student" | "employee";
            aboutPerson?: (about: string) => string;
        };
```

### Classes in TS
---
Unlike JS, we can add access modifiers to class member variables using the following modifiers (like C++):
- _`private`_: Visible only within the class.
- _`protected`_: Visible to members on the same class an dthe derived class.
- _`public`_ (default) : Visible to complete program.

### Generic Types
---
Used for creating functions, classes and type aliases for which we do not require to explicitly define types (similar to templates in C++). This is achieved putting a generic type inside angle-brackets.
> Example 1
```ts
function clone<T>(source: T) {
    const serialized = JSON.stringify(source);
    return JSON.parse(serialized);
}
```

_Explanation_: The above function has a generic type `T` for the parameter `sources` introduced to it  

<br>

> Example 2
```ts
interface data {
    data1: string;
    data2: number;
};

function clone<T, U>(source: T, options: U): T {
    const serialized = JSON.stringify(source);
    return JSON.parse(serialized);
}

const cloned = clone()
```