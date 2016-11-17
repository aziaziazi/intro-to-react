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

```javascript
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(<h1>Hello world</h1>, document.getElementById('app'));
```

## Advenced JSX

### class vs className

`class` is a reserved word in javascript. Therefore I **can't** use it in a JSX element. I have to use instead `className` that will be rendered as `class`. EG :

```JSX
<div className="myDiv">hey!</div>
```

will be rendered as :

```JSX
<div class="myDiv">hey!</div>
```

### Self Closing Tags

In HTML, the end-slash is optional :

```<br>``` works as good as ```<br />```

in JSX, the **end-slash is required** :

```JSX
// Works :
<br />
// Doesn't works :
<br>
```

### include Javascript inside JSX

Use curly braces marker```{}``` to inject javascript inside JSX. Eg :

```javascript
var math = <h1>2+3 = {2+3}</h1>;
```

The javascipt inside the culry braces is part of the same environment as the rest of the file > I can call variable declared outside of the JSX.

```javascript
var sideLength = "200px";

var panda = (
  <img
    src="images/panda.jpg"
    alt="panda"
    height={sideLength}
    width={sideLength} />
);
```

or with object properties

```javascript
var pics = {
  panda: "http://bit.ly/1Tqltv5",
  owl: "http://bit.ly/1XGtkM3",
  owlCat: "http://bit.ly/1Upbczi"
};

var panda = (
  <img
    src={pics.panda}
    alt="Lazy Panda" />
);

var owl = (
  <img
    src={pics.owl}
    alt="Unimpressed Owl" />
);

var owlCat = (
  <img
    src={pics.owlCat}
    alt="Ghastly Abomination" />
);
```

### Event Listeners in JSX

[list of valid events Listeners in React](https://facebook.github.io/react/docs/events.html#supported-events)

**ACHTUNG** lowercase for HTML, such as ``ònmouseover```, camelCase for JSX, such as ``onMouseOver```.

The event Listener value should be a function.