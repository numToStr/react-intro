---
titleTemplate: "%s - Vikas Rajbanshi"
favicon: /logo.svg
theme: bricks
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
hideInToc: true
---

<img alt="React Logo" src="/logo.svg" style="height:200px" class="m-auto" />

# Introduction to React

##

[https://react.dev](https://react.dev)

<!-- prettier-ignore-start -->

---
hideInToc: true
---

<!-- prettier-ignore-end -->

# We'll be covering

<Toc/>

---

# What is React?

##

React is a _library_ for making beautiful and interactive UI.

# What is not React?

##

It is **not a framework**, because it doesn't have an opinion on how you should handle your API, forms, routing etc,. However, it does give you the building block to do so, which enables everyone to have their own thoughts and solve those issue however they see fit.

---

# What is a Component?

##

Component is a single unit of UI. These are UI building blocks of the applications. It could be a stateless component or stateful component. You can define a component using a javascript `function`, use it a like a HTML element and add some `import`/`export` to reuse the component.

# What is JSX?

##

JSX is the syntax, used by React, to describe the structure/layout of the component. It looks like HTML but it is different in many ways. _In fact, it's all javascript till the end_.

Following is a simple example of valid React component where JSX starts from line 4-6.

```js
// file://src/component/example.jsx
export function Example() {
  return (
    <section>
      <p>Hello World!</p>
    </section>
  );
}
```

---

# Props, Children and Fragment

##

React compnents uses `props` to communicate and pass data from parent to child component. Props are defined as you would define normal HTML attribute, they can take string literals or any javascript value using curly braces `{ }`.

When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children` and `Fragment` (`<>...</>`) lets you group elements without a wrapper node.

```js {all} {maxHeight: '250px'}
function Parent() {
  const age = 99;
  return (
    <main>
      <Child name="John Doe" age={age}>
        Anything between tags is know as `children`
      </Child>
    </main>
  );
}

function Child(props) {
  return (
    <>
      <p>Name: {props.name}</p>
      <p>Age: {props.age}</p>
      {props.children}
    </>
  );
}
```

---

# Conditional Rendering

##

What if you want to show or hide or render a completely different component? You can use `if` statements, `&&` and `? :` (ternary) operator for that.

Let's understand by the following examples:

```js
function TodoItem1(props) {
  if (props.done) {
    return <s>1. First Item</s>;
  }
  return <p>1. First Item</p>;
}

function TodoItem2(props) {
  return props.done ? <s>1. First Item</s> : <p>1. First Item</p>;
}

function TodoItem3(props) {
  return (
    !props.hidden && (props.done ? <s>1. First Item</s> : <p>1. First Item</p>)
  );
}
```

---

# Working with Lists and Events

##

To render a similar component with different datasets, we use [javascript array methods](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array#) and most often `map()` method. We also need to give `key` prop to make each element unique in the list.

And to add event to your component, you first define a function and pass the function reference to the `onClick` props on the JSX element.

```js {all} {maxHeight: '280px'}
const todos = [
  { text: "Intro" },
  { text: "To", done: true },
  { text: "React" },
  { text: "!", hidden: true },
];

function TodoList() {
  return (
    <section>
      {todos.map((t) => (
        <TodoItem key={t.text} {...t} />
      ))}
    </section>
  );
}

function TodoItem(props) {
  const { hidden, done, text } = props;

  const handleClick = () => console.log(props);

  return (
    !hidden &&
    (done ? (
      <s onClick={handleClick}>{text}</s>
    ) : (
      <p onClick={handleClick}>{text}</p>
    ))
  );
}
```

---

# Managing Local State

##

Components often need state to store or change something on the UI. State can also be described as component-specific memory. And to do all that React provides [`useState`](https://react.dev/reference/react/useState) hook.

```js
import { useState } from "react";

export function Counter() {
  const [count, setCount] = useState(0);

  return (
    <section>
      <button aria-label="substract" onClick={() => setCount(count - 1)}>
        -
      </button>
      <span>{count}</span>
      <button aria-label="add" onClick={() => setCount(count + 1)}>
        +
      </button>
    </section>
  );
}
```

---

# Using Context

##

Passing props can become inconvenient if many components need the same information e.g., global theme. Context lets the parent component make some information available to any of its child component.

```js {all} {maxHeight:'340px'}
// file://src/theme.jsx
import { createContext } from "react";
export const ThemeContext = createContext();

// file://src/index.jsx
import { useState } from "react";
import { ThemeContext } from "./theme";
import { App } from "./App";

function Root() {
  const [theme, setTheme] = useState("dark");

  const toggleTheme = () => {
    setTheme(theme === "dark" ? "light" : "dark");
  };

  return (
    <ThemeContext.Provider value={[theme, toggleTheme]}>
      <App />
    </ThemeContext.Provider>
  );
}

// file://src/App.jsx
import { useContext } from "react";
import { ThemeContext } from "./theme";

export function App() {
  const [theme, toggleTheme] = useContext(ThemeContext);

  return (
    <section>
      <p>Theme is {theme}</p>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </section>
  );
}
```

---

# Async and Side Effects

##

Effects let you run some code after rendering so that you can synchronize your component with some system outside of React like an API request. Effect can be initiated by rendering or user event. React provides `useEffect` hook for side effects. Common examples for side effects are timers, accessing local storage, API request etc.

```js
import { useState, useEffect } from "react";
import { createConnection } from "./chat.js";

export function ChatRoom() {
  useEffect(() => {
    const connection = createConnection();
    connection.connect();

    return () => connection.disconnect();
  }, []);

  return <h1>Welcome to the chat!</h1>;
}
```

---

# Ecosystem Walkthrough

##

Components: [@mui/material](https://mui.com/), [@mantine/core](https://mantine.dev/)

Forms: [react-hook-form](https://www.react-hook-form.com/)

Routing: [react-router](https://reactrouter.com/en/main), [@tanstack/router](https://tanstack.com/router/v1)

Data Fetching: [react-query](https://tanstack.com/query/v3/), [swr](https://swr.vercel.app/)

State Management: [zustand](https://docs.pmnd.rs/zustand/getting-started/introduction), [recoil](https://recoiljs.org/)

Animation: [react-spring](https://www.react-spring.dev/), [framer-motion](https://www.framer.com/motion/)

3D/Games: [@react-three/fiber](https://docs.pmnd.rs/react-three-fiber/getting-started/introduction)

Mobile Apps: [react-native](https://reactnative.dev/), [expo](https://expo.dev/)

<!-- prettier-ignore-start -->

---
class: flex justify-center items-center
hideInToc: true
---

<!-- prettier-ignore-end -->

# Thank You!
