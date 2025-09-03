# Typescript Basics 
1. [Typescript Basics](#typescript-basics)
   1. [VS Code Typescript Setup](#vs-code-typescript-setup)
   2. [Create VS Code Typescript Project](#create-vs-code-typescript-project)
   3. [Recap of Typescript Basics](#recap-of-typescript-basics)
   4. [Other Primitive Examples](#other-primitive-examples)
   5. [Using Objects](#using-objects)
   6. [Using Mapped Types](#using-mapped-types)
   7. [Using Reusable Inteface Type for Object Typing](#using-reusable-inteface-type-for-object-typing)
   8. [Type Narrowing](#type-narrowing)
   9. [Using Generic Types](#using-generic-types)
2. [Using Typescript in React](#using-typescript-in-react)
   1. [Passing App.tsx Props to Component](#passing-apptsx-props-to-component)
   2. [Implement CourseGoal Component](#implement-coursegoal-component)
   3. [Using the Props Children Parameter](#using-the-props-children-parameter)
   4. [Using State](#using-state)
## VS Code Typescript Setup

Run the following command in a terminal window after creating a .ts file to provide typescript error checking. 

```
npm install typescript --save-dev
```

## Create VS Code Typescript Project

1. Run the command below in the project directory and then drag new folder into VS Code.

```
npm create vite@latest react-ts-basics
```

2. Open a terminal in the project and run the commands below.

```
npm install
npm run dev
```

3. Open a Simple Browser in VS Code and reference ```http://localhost:5173```

## Recap of Typescript Basics


In the example below, TS can infer as a ```string``` from the variable declaration, and the assignment of a number would be a **javascript error**.

```javascript
let userName = 'Max';
userName=35;  <<< this would be a javascript error 
```

Instead, the variable can be explicity defined using TS syntax which entails the use of a colon operator, followed by the type.

```javascript
let user: string = 'Greg';
user = 35; <<< This would be a typescript error
```

## Other Primitive Examples

```javascrirpt
let userAge = 34;
let age: number = 34;

let isValid = true;
let valid: boolean = true;
```

Where are variable can be of multiple types, use the union (pipe) operator

```javascript
let id: string | number;
id='abc';
id=12;
id=false; <<< typescript error
```

## Using Objects

```javascript
let userCreds: {
    userName: string;
    passwd: string;
} = {userName: 'Greg', passwd: 'ingadavidda'}
```
The above userCreds object type could be more easily define by the use of the Typescript ```type``` syntax.

## Using Mapped Types

[Typescript Mapped Reference](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

```javascript
type User = {
  name: string;
  age: number;
  isAdmin: boolean;
  id: string | number;
};

let user: User;

user = {
  name: 'Max',
  age: 34,
  isAdmin: true,
  id: 'abc', // or 123
};
```
The type can contain specific enumerations as shown in the example below, using a union operator

```javascript
type Role = 'admin' | 'user' | 'editor';

let role: Role; // 'admin', 'user', 'editor'

role = 'admin';
role = 'user';
role = 'editor';
role = 'abc'; <<< This would be a Typescript error
```

## Using Reusable Inteface Type for Object Typing

[Typescript Org Objects Reference](https://www.typescriptlang.org/docs/handbook/2/objects.html)

Although the ```type``` syntax can be used for objects, the ```inteface``` is typically used.

```javascript
interface Credentials {
  password: string;
  email: string;
}

let creds: Credentials;

creds = {
  password: 'abc',
  email: 'test@example.com',
};
```

## Type Narrowing

[Typescript Org Narrowing Reference](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

The example below provides a way prepend a number of spaces or a literal to an input string. It is an example of how to **narrow type input**, where the padding argument could be a number or a string. 

**Note**: The ```typeof``` is Javascript, not TS syntax.

```javascript
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}
```

## Using Generic Types

[Typescript Org Generics Reference](https://www.typescriptlang.org/docs/handbook/2/generics.html)

A type can be defined flexibly to support a variety of uses, as the following case illustrates. The custom generic type takes a **Generic** type of array.

```javascript
type DataStorage<T> = {
  storage: T[];
  add: (data: T) => void;
};
```
The generic type is then implemented by declaring to very different types of arrays as an argument.

```javascript
const textStorage: DataStorage<string> = {
  storage: [],
  add(data) {
    this.storage.push(data);
  },
};

// The User Type referenced below is in the Mapped types exmple

const userStorage: DataStorage<User> = {
  storage: [],
  add(user) {},
};
```

# Using Typescript in React 

## Passing App.tsx Props to Component

The goal is to display CourseGoal component from App.tsx. BTW, the App.tsx is invoked from src/main.tsx which is has the ```strict``` property wrapped around App.tsx. Below is the reference to the CourseGoal component inside of App.tsx.

```javascript
import CourseGoal from './components/CourseGoal.tsx'
export default function App() {
  return (
    <main>
      <CourseGoal title="Learn React + TS" description="Get a Jump Start on React and Typescript"/>
    </main>
  );
}
```
## Implement CourseGoal Component

1. Create a folder named ```src/components```
2. Create ```CourseGoal.tsx``` in the components directory.
3. Define CourseGoals.tsx as shown below.
```javascript
export default function CourseGoal(props: {title: string; description:string;}) {
    const title=props.title;
    const description=props.description;
    return <article>
        <div>
            <h2>{title}</h2>
            <p>{description}</p>
        </div>
        <button>Delete</button>
    </article>
}
```
4. Cleanup the ```props``` object reference by using javascript destructuring but still retaining the use of TS to define the object.

```javascript
export default function CourseGoal({title, description}: {
    title: string;
    description: string
}) {
    return <article>
        <div>
            <h2>{title}</h2>
            <p>{description}</p>
        </div>
        <button>Delete</button>
    </article>
}
```

5. Another refactoring that is a good practice is to use an ```Interface``` to define the course goal props.
 
```javascript
interface CourseGoalProps {
    title: string;
    description: string;
}

export default function CourseGoal({title, description}: CourseGoalProps) {
    return <article>
        <div>
            <h2>{title}</h2>
            <p>{description}</p>
        </div>
        <button>Delete</button>
    </article>
}
```

## Using the Props Children Parameter

The ```ReactNode``` type is a TS type defined as a dependency in the package.json file from the 3rd line shown below.

```
  "devDependencies": {
    "@eslint/js": "^9.33.0",
    "@types/react": "^19.1.10",
    ...
```

The children parameter is available on every component invocation using props. If there is JSX that is wrapping children properties in the component passing props. Shown below is the description of the course, wrapped as JSX children prop.

```javascript
import CourseGoal from './components/CourseGoal.tsx'
export default function App() {
  return (
    <main>
      <CourseGoal title="Learn React + TS">
        Get a Jump Start on React and Typescript
      </CourseGoal>
    </main>
  );
}
```

The CourseGoal component now must reference ```children```, not ```description``` as a parameter. Note the use of a special TS type of ```ReactNode``` to define it. It is a best practice on the import to prepend ```type```.

```javascript
import { type ReactNode } from "react";

interface CourseGoalProps {
    title: string;
    children: ReactNode;
}

export default function CourseGoal({title, children}: CourseGoalProps) {
    return <article>
        <div>
            <h2>{title}</h2>
            <p>{children}</p>
        </div>
        <button>Delete</button>
    </article>
}
```
## Using State

When the ```useState``` hook is specified in either a .tsx or .jsx file, an import is created for it. Since TS is to provide type safety, the syntax employed for the useState is different than JS of course. 

In the example below, a type of ```Goal``` was defined so the useState hook could correctly specify an array of Goal objects in the **angled brackets**. In JS, simple **square brakets** in the parentheses was all that was needed.

```javascript
import { useState } from 'react';
...
export default function App() {
  type Goal = {
    name: string;
    description: string;
    id: number;
  }
  const [goals, setGoals] = useState<Goal[]>([]);
```
