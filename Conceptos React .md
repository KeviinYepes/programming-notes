# What is React?
React is an open-source JavaScript library designed to build user interfaces.

# Advantages of React

1. Easy to learn.  
2. Reusable components.  
3. Build dynamic applications.  
4. Good performance.  

# Basic concepts of React

## Componentes
A component is an independent and reusable part of the user interface.

###  Functional Component 

These are JavaScript functions that return a React JSX (JavaScript XML) element.  
*Example*  
````bash
function Greeting(props) {
  return <h1>¡Hola, {props.name}!</h1>;
}
````
### Class Component
These are ES6 (Modern JavaScript) classes that return a JSX (JavaScript XML) element.
*Example*
````bash
class saludo extends React.Component {
  render() {
        return <h1>¡Hola, {this.props.nombre}!</h1>
    }
}
````

## What is JSX (JavaScript XML)?
A syntax extension for JavaScript, created by Facebook to be used with the React framework. It allows you to write HTML-like code inside your JavaScript files. Its main goal is to make building user interfaces easier by combining rendering logic with interface logic in "components." Although it’s not mandatory, JSX improves code readability and is widely used by React developers to create interface elements and components.

## Advantages of JSX
1. Easier-to-visualize structure  
2. More helpful errors and warnings  

### Basic JSX Example
````bash
const element = <h1>Hello World!</h1>
````


## Props
In React, props (short for "properties") are a mechanism to pass data from a parent component to a child component, enabling communication and the creation of reusable components.

### Características principales de las props:
1. One-way communication: data always flows from a parent component to its child components, never the other way around.
2. Immutability: props are read-only; the child component cannot change the values it receives from the parent.

## Estado
setState() schedules an update to a component’s state object. When the state changes, the component responds by re-rendering.

## Hook
Hooks are special functions that allow functional components to access React features that were previously only available to class components, such as state and lifecycle, without needing to write a class.

## Event Listener
A function that runs when a specific event occurs.

