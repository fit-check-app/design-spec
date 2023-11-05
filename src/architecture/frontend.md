# Frontend architecture overview

## Programming language

The candidate languages for the user facing application of this project are 

- JavaScript
- TypeScript

JavaScript offers the following advantages

1) Dynamic Typing: JavaScript is dynamically typed, which means you don't need to explicitly declare variable types. Types are determined at runtime.
2) Interpreted Language: JavaScript is an interpreted language that is executed in the browser or on a server using a JavaScript runtime (e.g., Node.js).
3) Loose Typing: JavaScript is loosely typed, allowing you to perform implicit type conversions. This can lead to unexpected behavior in some cases.
4) ECMAScript Standard: JavaScript follows the ECMAScript standard, with different versions (e.g., ES5, ES6, ES7, etc.) introducing new features and improvements over time.
5) No Static Typing: JavaScript lacks static typing support, which can make it challenging to catch type-related errors during development.
6) Large Ecosystem: JavaScript has a vast ecosystem of libraries, frameworks, and tools, making it a popular choice for web development.

TypeScript offers the following advantages

1) Static Typing: TypeScript introduces static typing, allowing you to declare types for variables, function parameters, and return values. This helps catch type-related errors at compile time.
2) Superset of JavaScript: TypeScript is a superset of JavaScript, meaning that any valid JavaScript code is also valid TypeScript code. You can gradually adopt TypeScript in existing projects.
3) Compiled Language: TypeScript code is transpiled (compiled) into JavaScript, which can be executed in browsers or server-side environments. The transpiler enforces type checking and generates clean, readable JavaScript code.
4) Strict Typing Rules: TypeScript enforces stricter typing rules, which can help prevent common programming errors and enhance code quality and maintainability.
5) Improved Tooling: TypeScript comes with enhanced tooling, including code editors like Visual Studio Code, which provides features like autocompletion, error checking, and code navigation based on type information.
6) Interfaces and Generics: TypeScript supports interfaces and generics, making it easier to define and work with complex data structures and reusable code components.
7) IDE Support: TypeScript provides rich support for Integrated Development Environments (IDEs), making it easier to work with larger codebases and improving developer productivity.

## Frontend framework

The anticpated web framework to use to build the user interface is [Vue](https://vuejs.org). Vue boasts the following advantages on the homepage

- Approachable: Builds on top of standard HTML, CSS and JavaScript with intuitive API and world-class documentation.
- Performant: Truly reactive, compiler-optimized rendering system that rarely requires manual optimization.
- Versatile: A rich, incrementally adoptable ecosystem that scales between a library and a full-featured framework.

This framework seems to support TypeScript out of the box and can incorporate the npm ecosystem.

## Breaking the UI into reusable components

### Clothing Items Cards

The cards that show simple views of a clothing item are structure similarly as follows

- Image of the item in question
- Details about availability and last worn
- Action buttons to 
    - Open the detailed view 
    - Perform a quick action (wear today)

### Search and navigation

This mostly consists of a screen title and a search bar as specified [here](../interaction/clothing-item-management.md)

- A screen name ("My wardrobe") + a search bar
- A nest item view ("My wardrobe > item name") + floating action button pertaining to the view 

### Detailed item view

> Section to be clearly separated and composed into a larger parent component

#### Required section

- A place for an image of the item 
- A place the the required fields as specified [here](../interaction/clothing-item-management.md)

#### Optional section

- A place for optional fields to be laid out
- Ideally separated by vertical separator
    - Economics (brand, cost, purchase date)
    - Cloth details (Colore, Size, Material)
    - Setting (Season tags, Occasion tags, addtional notes)
