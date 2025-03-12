# Chapter 1 - About React and Next.js

## What is React?

React is a JavaScript library for building interactive user interfaces.

By library, we mean React provides helpful functions (APIs) to build UI, but leaves it up to the developer where to use those functions in their application.

## What is Next.js?

Next.js is a flexible React framework that gives you building blocks to create fast, full-stack web applications.

By framework, we mean Next.js handles the tooling and configuration needed for React, and provides additional structure, features, and optimizations for your application.

You can use React to build your UI, then incrementally adopt Next.js features to solve common application requirements such as routing, data fetching, and caching - all while improving the developer and end-user experience.



# Chapter 2 - Rendering User Interfaces (UI)

## What is the DOM?

The DOM is an object representation of the HTML elements. It acts as a bridge between your code and the user interface, and has a tree-like structure with parent and child relationships.

You can use DOM methods and JavaScript, to listen to user events and manipulate the DOM by selecting, adding, updating, and deleting specific elements in the user interface. 

DOM manipulation allows you to not only target specific elements, but also change their style and content.



# Chapter 3 - Updating UI with Javascript

## HTML vs. the DOM

Updating the DOM with plain JavaScript is very powerful but verbose. As the size of an app or team grows, it can become increasingly challenging to build applications this way.

## Imperative vs. declarative programming

In other words, **imperative programming** is like giving a chef step-by-step instructions on how to make a pizza.

**Declarative programming** is like ordering a pizza without being concerned about the steps it takes to make the pizza. 🍕

## React: A declarative UI library

As a developer, you can tell React what you want to happen to the user interface, and React will figure out the steps of how to update the DOM on your behalf.



# Chapter 4 - Getting Started with React

To use React in your newly created project, load two React scripts from an external website called [unpkg.com]:
* react is the core React library.
* react-dom provides DOM-specific methods that enable you to use React with the DOM.

Add the ReactDOM.createRoot() method to target a specific DOM element and create a root to display your React Components in. Then, add the root.render() method to render your React code to the DOM.

This will tell React to render our <h1> title inside our #app element.

```html/jsx
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script>
      const app = document.getElementById('app');
      const root = ReactDOM.createRoot(app);
      root.render(<h1>Develop. Preview. Ship.</h1>);
    </script>
  </body>
</html>
```

If you try to run this code above in the browser, you will get a syntax error because <h1>...</h1> is not valid Javascript. This piece of code is JSX.

## What is JSX?

JSX is a syntax extension for JavaScript that allows you to describe your UI in a familiar HTML-like syntax.

The nice thing about JSX is that apart from following [three JSX rules], you don't need to learn any new symbols or syntax outside of HTML and JavaScript.

But browsers don't understand JSX out of the box, so you'll need a JavaScript compiler, such as a Babel, to transform your JSX code into regular JavaScript.

## Adding Babel to your project

To add Babel to your project, copy and paste the following script in your index.html file:

```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```

In addition, you will need to inform Babel what code to transform by changing the script type to `type=text/jsx`.

```html
<script type="text/jsx">
```



# Chapter 5 - Building UI with Components

## Components

Components allow you to build self-contained, reusable snippets of code. If you think of components as **LEGO bricks**, you can take these individual bricks and combine them together to form larger structures.

This modularity allows your code to be more maintainable as it grows because you can add, update, and delete components without touching the rest of our application.

### Creating components

A component is a function that returns UI elements.

First, React components should be capitalized to distinguish them from plain HTML and JavaScript.

Second, you use React components the same way you'd use regular HTML tags, with angle brackets <>

Inside the return statement of the function, you can write JSX:
```html/jsx
<script type="text/jsx">

    const app = document.getElementById("app")
 
    function Header() {
        return <h1>Develop. Preview. Ship.</h1>;
    }
 
    const root = ReactDOM.createRoot(app);
    root.render(<Header />);

</script>
```

### Nesting components

You can nest React components inside each other like you would regular HTML elements.

This modular format allows you to reuse components in different places inside your app.

```jsx
function HomePage() {
    return (
        <div>
            <Header />
        </div>
    );
}

const root = ReactDOM.createRoot(app);
root.render(<HomePage />);
```



# Chapter 6 - Displaying Data with Props

You can pass pieces of information as properties to React components. These are called props.

Similar to a JavaScript function, you can design components that accept custom arguments (or props) that change the component's behavior or what is visibly shown when it's rendered to the screen. Then, you can pass down these props from parent components to child components.

## Using Props

In your HomePage component, you can pass a custom title prop to the Header component, just like you'd pass HTML attributes

if you console.log() props, you can see that it's an object with a title property.

Since props is an object, you can use [object destructuring] to explicitly name the values of props inside your function parameters.

## Using variables in JSX

To use the title prop, add curly braces {}. These are a special JSX syntax that allows you to write regular JavaScript directly inside your JSX markup.

You can think of curly braces as a way to enter "JavaScript land" while you are in "JSX land". 

```jsx
function Header({ title }) {
    console.log(title);
    return <h1>{title}</h1>;
}
```

## Iterating through lists

You can use array methods to manipulate your data and generate UI elements that are identical in style but hold different pieces of information.

React needs something to uniquely identify items in an array so it knows which elements to update in the DOM. It's recommended to use something guaranteed to be unique, like an item ID.

You can then use the array.map() method to iterate over the array and use an arrow function to map a name to a list item:
```jsx
<div>
    <Header title="Aprendendo os fudamentos do React" />
    <ul>
        {names.map((name) => (
            <li>{name}</li>
        ))}
    </ul>
</div>
```



# Chapter 7 - Adding Interactivity with State

## Listening to events

In React, event names are `camelCased`.

```jsx
<button onClick={}>Like</button>
```

## Handling events

You can define a function to "handle" events whenever they are triggered. 

```jsx
<button onClick={handleClick}>Like</button>
```

## State and hooks

React has a set of functions called [hooks]. Hooks allow you to add additional logic such as state to your components.

You can think of state as any information in your UI that changes over time, usually triggered by user interaction.

The React hook used to manage state is called: useState(). It returns an array, and you can access and use those array values inside your component using array destructuring:
* The first item in the array is the state value, which you can name anything. It's recommended to name it something descriptive.
* The second item in the array is a function to update the value. You can name the update function anything, but it's common to prefix it with set followed by the name of the state variable you're updating.

You can also take the opportunity to add the initial value of your state to 0:

```jsx
const [likes, setLikes] = React.useState();
```

Finally, you can call your state updater function, `setLikes` in your component, let's add it inside the `handleClick()` function you previously defined:

```jsx
const [likes, setLikes] = React.useState(0);
 
function handleClick() {
    setLikes(likes + 1);
}

return (
    <div>
        <button onClick={handleClick}>Likes ({likes})</button>
    </div>
);
```

Clicking the button will now call the `handleClick` function, which calls the `setLikes` state updater function with a single argument of the current number of likes + 1.

> Note: Unlike props which are passed to components as the first function parameter, the state is initiated and stored within a component. You can pass the state information to children components as props, but the logic for updating the state should be kept within the component where state was initially created.

## Managing state

This was only an introduction to state, and there's more you can learn about managing state and data flow in your React applications. To learn more, we recommend you go through the [Adding Interactivity] and [Managing State] sections in the React documentation.

> Additional Resources:
> [State: A component's memory]
> [Meet your first hook]
> [Responding to Events]



# Chapter 8 - From React to Next.js

So far, we explored how you can get started with React. This is what the final code looked like. If you're starting from here, paste this code into an `index.html` file in your code editor.

```jsx
<script async="false" type="text/jsx">
    const app = document.getElementById('app');

    function Header({ title }) {
        return <h1>{title ? title : "Default title"}</h1>
    }
    
    function HomePage({ title }) {
        const names = ["Ada Lovelace", "Grace Hopper", "Margaret Hamilton"]
    
        const [likes, setLikes] = React.useState(0)
    
        function handleClick() {
            setLikes(likes + 1)
        }
    
        return (
            <div>
                <Header title={title} />
                
                <ul>
                    {names.map((name) => (
                    <li key={name}>{name}</li>
                    ))}
                </ul>
        
                <button onClick={handleClick}>Like ({likes})</button>
            </div>
        )
    }

    const root = ReactDOM.createRoot(app);
    root.render(<HomePage title="Aprendendo os fudamentos do React" />);
</script>
```

While React excels at building UI, it does take some work to independently build that UI into a fully functioning scalable application. There are also newer React features, like Server and Client Components, that require a framework. 

The good news is that Next.js handles much of the setup and configuration and has additional features to help you build React applications.



# Chapter 9 - Installing Next.js

When you use Next.js in your project, you do not need to load the `react` and `react-dom` scripts from [unpkg.com] anymore. Instead, you can install these packages locally using `npm` or your preferred package manager.

> Note: To use Next.js, you will need to have Node.js version **18.17.0** or above installed on your machine ([see minimum version requirement]), you can [download it here].

To do so, create a new file in the same directory as your `index.html` file, called `package.json` with an empty object `{}`.

In your [terminal], run the following command in the root of your project:

```Terminal
npm install react@latest react-dom@latest next@latest
```

Don't worry if you're on later versions than the ones shown above, as long as you have the next, react, and react-dom packages installed, you're good to go.

You will also notice a new file called package-lock.json file that contains detailed information about the exact versions of each package.

Jumping back to the index.html file, you can delete the following code:
1. The <html> and <body> tags.
2. The <div> element with the id of app.
3. The react and react-dom scripts since you've installed them with NPM.
4. The Babel script because Next.js has a compiler that transforms JSX into valid JavaScript browsers can understand.
5. The <script type="text/jsx"> tag.
6. The document.getElementById() and ReactDom.createRoot() methods.
7. The React. part of the React.useState(0) function

After deleting the lines above, add the following import to the top of your file:

```jsx
import { useState } from 'react';
```

Your code should look like this:

```jsx
import { useState } from 'react';

function Header({ title }) {
    return <h1>{title ? title : "Default title"}</h1>
}

function HomePage({ title }) {
    const names = ["Ada Lovelace", "Grace Hopper", "Margaret Hamilton"]

    const [likes, setLikes] = useState(0)

    function handleClick() {
        setLikes(likes + 1)
    }

    return (
        <div>
            <Header title={title} />
            
            <ul>
                {names.map((name) => (
                <li key={name}>{name}</li>
                ))}
            </ul>
    
            <button onClick={handleClick}>Like ({likes})</button>
        </div>
    )
}
```

The only code left in the HTML file is JSX, so you can change the file type from `.html` to `.js` or `.jsx`.

## Creating your first page

Next.js uses file-system routing. This means that instead of using code to define the routes of your application, you can use **folders** and **files**.

Here's how you can create your first page in Next.js:
1. Create a new folder called app and move the `index.js` file inside it.
2. Rename your `index.js` file to page.js. This will be the main page of your application.
3. Add `export default` to your `<HomePage>` component to help Next.js distinguish which component to render as the main component of the page.

```jsx
import { useState } from 'react';

function Header({ title }) {
    return <h1>{title ? title : "Default title"}</h1>
}

export default function HomePage({ title }) {
    // ...
}
```

## Running the development server

Next, let's run your development server so you can see the changes in your new page while developing. Add a "next dev" script to your package.json file:

```json
{
  "scripts": {
    "dev": "next dev"
  },
  // ...
}
```

Check what happens by running `npm run dev` in your terminal. You'll notice two things:
1. An error because Next.js uses React Server Components and Server Components don't support `useState`, so you'll need to use a Client Component instead.
2. A new file called `layout.js`, it was automatically created inside the `app` folder. This is the main layout of your application. You can use it to add UI elements that are shared across all pages (e.g. navigation, footer, etc).

## Summary

Looking at the migration so far, you may already be getting a sense of the benefits of using Next.js:
* You removed the React and Babel scripts; a taste of the complex tooling and configuration you no longer have to think about.
* You created your first page.

Additional Reading:
* [Next.js Routing Fundamentals]
* [Defining Routes]
* [Pages and Layouts]

# Chapter 10 - Server and Client Components

To understand how Server and Client Components work, it's helpful to be familiar with two foundational web concepts:
* The environments your application code can be executed in: the server and the client.
* The network boundary that separates server and client code.

## Server and Client Environments

In the context of web applications:
* The **client** refers to the browser on a user’s device that sends a request to a server for your application code. It then turns the response it receives from the server into an interface the user can interact with.
* The **server** refers to the computer in a data center that stores your application code, receives requests from a client, does some computation, and sends back an appropriate response.

Therefore, the code you write for the server and the client is not always the same. Certain operations (e.g. data fetching or managing user state) are better suited for one environment over the other.

## Network Boundary

The **Network Boundary** is a conceptual line that separates the different environments.

In React, you choose where to place the network boundary in your component tree. For example, you can fetch data and render a user's posts on the server (using Server Components), then render the interactive LikeButton for each post on the client (using Client Components).

After Server Components are rendered, a special data format called the **React Server Component Payload (RSC)** is sent to the client. The RSC payload contains:
1. The rendered result of Server Components.
2. Placeholders (or holes) for where Client Components should be rendered and references to their JavaScript files.

React uses this information to consolidate the Server and Client Components and update the DOM on the client.

## Using Client Components

As you learned in the last chapter, Next.js uses Server Components by default - this is to improve your application's performance and means you don't have to take additional steps to adopt them.

Looking back at the error in your browser, Next.js is warning you that you're trying to `useState` inside a Server Component. 

You can fix this by moving the interactive "Like" button to a Client Component:
1. Create a new file called `like-button.js` inside the `app` folder that exports a `LikeButton` component.
2. Move the `<button>` element and the `handleClick()` function from `page.js` to your new `LikeButton` component.
3. Next, move the `likes` state and the import.
4. Now, to make the `LikeButton` a Client Component, add the React `'use client'` directive at the top of the file. This tells React to render the component on the client.

```jsx

'use client';

import { useState } from 'react';
 
export default function LikeButton() {
  const [likes, setLikes] = useState(0);
 
  function handleClick() {
    setLikes(likes + 1);
  }
 
  return <button onClick={handleClick}>Like ({likes})</button>;
}

```

Back in your `page.js` file, import the `LikeButton` component into your page:

```jsx
import LikeButton from './like-button';
```

Save both files and view your app in the browser. Now that there are no errors, once you make changes and save, you should notice the browser automatically updates to reflect the change.

This feature is called [Fast Refresh]. It gives you instantaneous feedback on any edits you make and comes pre-configured with Next.js.

## Summary

To recap, you learned about the server and client environments and when to use each. You also learned that Next.js uses React Server Components by default to improve performance, and how you can opt into Client Components to smaller parts of your UI interactive.

**Additional Reading**
There's a lot more to learn about Server and Client Components. Here are some additional resources:
* [Server Components Docs]
* [Client Component Docs]
* [Composition Patterns]
* [The "use client" Directive]
* [The "use server" Directive]



[//]: # (Links de referência)

[three JSX rules]: <https://react.dev/learn/writing-markup-with-jsx#the-rules-of-jsx>

[unpkg.com]: <https://unpkg.com/>

[object destructuring]: <https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment>

[hooks]: <https://react.dev/learn>

[Adding Interactivity]: <https://react.dev/learn/adding-interactivity>

[Managing State]: <https://react.dev/learn/managing-state>

[State: A component's memory]: <https://react.dev/learn/state-a-components-memory>

[Meet your first hook]: <https://react.dev/learn/state-a-components-memory#meet-your-first-hook>

[Responding to Events]: <https://react.dev/learn/responding-to-events>

[see minimum version requirement]: <https://nextjs.org/docs/getting-started/installation>

[download it here]: <https://nodejs.org/en/>

[terminal]: <https://code.visualstudio.com/docs/terminal/basics>

[Next.js Routing Fundamentals]: <https://nextjs.org/docs/app/building-your-application/routing>

[Defining Routes]: <https://nextjs.org/docs/app/building-your-application/routing/defining-routes>

[Pages and Layouts]: <https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts>

[Fast Refresh]: <https://nextjs.org/docs/architecture/fast-refresh>

[Server Components Docs]: <https://nextjs.org/docs/app/building-your-application/rendering/server-components>

[Client Component Docs]: <https://nextjs.org/docs/app/building-your-application/rendering/client-components>

[Composition Patterns]: <https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns>

[The "use client" Directive]: <https://react.dev/reference/react/use-client>

[The "use server" Directive]: <https://react.dev/reference/react/use-server>