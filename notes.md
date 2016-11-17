# Intro

React JS is a Javascript library developed by engineers at Facebook.
- **Fast**, thanks to the use of [virtual DOM](https://www.codecademy.com/articles/react-virtual-dom).
- **Modular** : i can write many small, reusable files.
- **Scalable** : it perform best to display a lot of changing data.
- **Flexible** : [can be use for projects that have nothing to do with web app](https://medium.mybridge.co/22-amazing-open-source-react-projects-cb8230ec719f#.1780zdlyj)

# Intro to JSX

JSX is a **syntax extension** for JavaScript : can't be read by Browser, should be compiled first.

## JSX Element

Looks a lot like an HTML element.

```JSX
<h1>Hello World</h1>
```

It's a javascipt expression, so it can go in a variable, passed to a function, stored in an object or array...

```javascript
const toDo = {
	first
}
```

JSX elements can have **attribute**, just like HTML elements:


```JSX
<h1 id="title">Hello World</h1>
```

if multiple lines, JSX elements must be wrapped in several lines with a parenthesis. Also, JSX elements must have exactly one outermost element.

will **not** work :

```JSX
var myList = (
	<li>Learn React</li>
	<li>Make a great app</li>
	<li>Be hired</li>
);
```

will work :

```JSX
var myList = (
	<ol>
		<li>Learn React</li>
		<li>Make a great app</li>
		<li>Be hired</li>
	</ol>
);
```

## Render a JSX element

I can use '.render(JSX, container)` to render *JSX* inside the *container*

```javascipt
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(<h1>Hello world</h1>, document.getElementById('app'));
```
