---
theme: bricks
class: text-center
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
transition: slide-left
---

<svg xmlns="http://www.w3.org/2000/svg" viewBox="-11.5 -10.23174 23 20.46348" height="200" class="m-auto">
    <title>React Logo</title>
    <circle cx="0" cy="0" r="2.05" fill="#61dafb"/>
    <g stroke="#61dafb" stroke-width="1" fill="none">
        <ellipse rx="11" ry="4.2"/>
        <ellipse rx="11" ry="4.2" transform="rotate(60)"/>
        <ellipse rx="11" ry="4.2" transform="rotate(120)"/>
    </g>
</svg>

# Introduction to React

##

[https://react.dev](https://react.dev)

---

# We'll be covering

- What is (not) React?
- What is a Component and JSX?
- Passing Data Around
- Conditional Rendering
- Working with Lists and Events
- Managing Local State
- Using Context
- Async and Side Effects
- Ecosystem Walkthrough

---

# What is React?

##

React is a **UI library** for making beautiful and interactive web applications.

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

# Passing data around

##

React compnents uses `props` to communicate and pass data from parent to child component. Props are defined as you would define normal HTML attribute, they can take string literals or any javascript value using curly braces `{ }`.

```js
function Parent() {
  const age = 99;
  return (
    <main>
      <Child name="John Doe" age={age} />
    </main>
  );
}

function Child(props) {
  return (
    <section>
      <p>Name: {props.name}</p>
      <p>Age: {props.age}</p>
    </section>
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

To render a similar component with different datasets, we use [javascript array methods](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array#) and most often `map()` method.

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
        <TodoItem {...t} />
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

---

# Ecosystem Walkthrough