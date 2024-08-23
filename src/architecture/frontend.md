# Front end architecture overview

## Programming language

The languages for the user facing application of this project are as follows:

- HTML
- CSS
- TypeScript

### HTML

Since, the user facing application is intended to be a web application, using HTML is the natural choice. HTML is the standard markup language on the web. While it may not be used explicitly in the project, its roots are fundamental when utilizing other web technologies like JSX.

### CSS

To help with application styling and responsiveness, CSS will be used. CSS is the standard language for adding style to web pages. Once again, it may not be used explicitly in the project since modern CSS alternatives like Tailwind CSS may abstract away details. Intimate knowledge of CSS will still be incredibly useful.

### TypeScript

TypeScript will be used to write the logic and interactivity of the web application. TypeScript is a superset of JavaScript that adds static typing to the language. This will help catch errors early and make the project more enjoyable to work on.

## Frontend frameworks

### React

React is a JavaScript library for building web interfaces. It is quite bare bones so it may be better to use a more feature rich framework like [Next.js](https://nextjs.org) or [Remix](https://remix.run). These frameworks will help with routing, data fetching, and components so that focus can be on the application itself.

### Tailwind CSS

Tailwind CSS is a utility-first CSS framework that helps style web pages without writing custom CSS. It is a great choice for rapid prototyping and development. It provides a lot of utility classes that can be used to style components and make responsive designs. The scope of the project should be defined well enough so that custom CSS is not needed.

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

- Section to be clearly separated and composed into a larger parent component

#### Required section

- A place for an image of the item 
- A place the the required fields as specified [here](../interaction/clothing-item-management.md)

#### Optional section

- A place for optional fields to be laid out
- Ideally separated by vertical separator
    - Economics (brand, cost, purchase date)
    - Cloth details (Colore, Size, Material)
    - Setting (Season tags, Occasion tags, addtional notes)
