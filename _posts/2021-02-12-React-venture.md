---
layout: post
title: A journey in React ⚛ territory
color: rgb(96,216,251)
tags: [js]
---

[React](https://reactjs.org/) is a well known javascript library for building user interfaces.
React is part of the Facebook Open source, and has a very good documentation to [learn](https://reactjs.org/docs/introducing-jsx.html) 📚!

You can get started with your own React application using [Create React App](https://github.com/facebook/create-react-app).
It is a boilerplate app creator, you can use it via:

```bash
npx create-react-app <your app name>
```

And here you go, a folder with your app name has been created and you can start running the default page!

# Concept

## React, JSX and Components

Basically like many other libraries, framework out there, _components_ are abstracted part containing their own logic using javascript and viw using HTML markups. 

React goes further with **JSX** (Javascript XML), because even inside of the component markup and logic are intertwined.
The example given is:

```js
const element = <h1>Hello, world!</h1>;
```

It is a perfectly valid React component, there's no real logic into it. However it's mixing javascript notation with HTML markups.
When used in browser this kind of _"new era javascript"_ is not always understood by the browser. Heck even the color highlighting is bugging here!

> That's why there are some "compiler" like [babel](https://babeljs.io/) that transforms it into a more "vanilla javascript"

To bring it back to our point.
The concept is fairly simple, React will compute the DOM (Document Object model, the web page) based on the component logic and markup and rewrite the updated parts.

## Render with ReactDOM

Here would be another example of how to render something with React if you haven't already checked the
other ones on the [React doc](https://reactjs.org/docs/rendering-elements.html)
First we'll define another tiny component that takes a parameter:

```js
const Greeting = ({ greeting }) => <h1>{greeting.text}</h1>;
```

Great, it will render the text into what's commonly known as a title with the `<h1>` markup.
To render it we would do:

{% raw %}
```js
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <React.StrictMode>
    <Greeting greeting={{ text: 'Welcome to React' }} />
  </React.StrictMode>,
  document.getElementById('root')
);
```
{% endraw %}

That means, ReactDOM will look for an element with the `root` id and attach our component to it.
The _React.StrictMode_ helps you write better components by highlighting in the console any potential error generated by the underneath components.
In the end it will amount to something like:

```html
<div id='root'>
    <h1>Welcome to React</h1>
</div>
```

That's how your react JSX gets integrated and rendered in the DOM. 
And now it is time to dive further into the possibilities of React components.

# React Component

While exploring React and stumbling upon issues, I came across [Robin Wieruch's blog](https://www.robinwieruch.de/react-component-types) 
and it is full of nice article on how to do about anything in React 👌 so check it out for some real cool stuff.
Now let's see how to write some components.

## Vocabulary

Here is to layout some of the vocabulary around components that are peculiar to [javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) or React.
So let's just add some simple definition on those:

  - **props**: Props are like the parameters you pass down a function, or component. It stands for property.
  In React they must be [read only](https://reactjs.org/docs/components-and-props.html#props-are-read-only), you don't modify the props directly.
  - **state**: Talking about stateful and stateless, state represents the current value(s) of a component. The state is like a _"private member"_ and can't be updated [directly](https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly).
  You need to use something like `setState()` so each time you update the state, it will render your component with its new value.
  - **lifecycle**: This is handling of the React component that will go through different stage from before it appears in the DOM, to being updated until after it gets removed.
  The React component [API](https://fr.reactjs.org/docs/react-component.html) provides methods to control the behavior in each of those stage

There's a nice interactive diagram available [online](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) that shows the common lifecycle.

{% include aligner.html images="react-lifecycle.png" column=1 %}

As you can see those are the main steps, but there are also some less commonly used lifecycle steps for advanced usage. 
Now that we have more perspective, let's dive into the main components type and see how it work together.

## Functional components

Usually those components are pretty straightforward and doesn't require much logic to them.
They leverage the abstraction / encapsulation permitted by react to reduce the size of bigger component by breaking them down into small pieces.
With dumb components that bare no logic like:

```js
const HelloWorld = () => <p> Hello World! </p>
```

Or if you rememberin the first part; the `Greeting` above was another example of a very simple functional component.
Using React [hooks](https://reactjs.org/docs/hooks-rules.html#gatsby-focus-wrapper) you can make your component more intelligent:

```js
import React, { useEffect, useState } from 'react';

export default function FunctionalComponent() {
  const [value, setValue] = useState('default value');

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts/')
      .then(res => res.json())
      .then((result) => {
        setValue(result[0].title);
        }
      )
  }, []);

  return <p>{value ? value : 'not received'}</p>
}
```

This component use the `useEffect` hook to update itself (via fetching some info) and we use the `useState` hook to create and save the state of the component.

You can also have high order component when for example you have a list for data for which each will be rendered via another component.
So once you mastered the syntax, you'll be able to go fancy with those.


## Class Component

The class component makes up for more controlled (handle props, state and lifecyle methods) and verbose component.
Here is an example component with it's most used capabilities:


```js
export default class ClassComponent extends React.Component {
  constructor(props) {
    super(props); 
    this.state = { value: 'default value' };
  }

  componentDidMount() {
    console.log('I was mounted on screen');
    this.setState({ value: this.props.value });
  }

  componentWillUnmount() {
    console.log('better do some clean up to avoid a memory leak')
  }

  render() {
    return <p> Render my {this.state.value} </p>
  }
}
```

The props, and state gets to be managed within the constructor. And you have access to the _lifecycle methods_, those methods
are called automatically. A component is said to be mounted when it's in the DOM.

You can use this component easily passing down some props name value using:

```html
<ClassComponent value={'props value'}/>
```

Let's follow how the props gets handled:

  1. You can see that `value={'props value'}` is being passed to the component when using it.
  2. The `props` that get passed to the constructor then looks like that: 
  ```json
{ props: { value: 'props value' } }
  ``` 
  3. And the `super(props)` will copy this props to `this.props` so we can access it in our component.

You can see how to convert a functional component to a class component on the [react doc](https://reactjs.org/docs/state-and-lifecycle.html#converting-a-function-to-a-class).
The best way to learn is always to try it out by yourself so let's get coding 👩‍💻👨‍💻!