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

```jsx
<h1>Hello World</h1>
```

It's a javascipt expression, so it can go in a variable, passed to a function, stored in an object or array...

```javascript
const toDo = {
  first
}
```

JSX elements can have **attribute**, just like HTML elements:


```jsx
<h1 id="title">Hello World</h1>
```

if multiple lines, JSX elements must be wrapped in several lines with a parenthesis. Also, JSX elements must have exactly one outermost element.

will **not** work :

```javascript
var myList = (
  <li>Learn React</li>
  <li>Make a great app</li>
  <li>Be hired</li>
);
```

will work :

```javascript
var myList = (
  <ol>
    <li>Learn React</li>
    <li>Make a great app</li>
    <li>Be hired</li>
  </ol>
);
```

## Render a JSX element

I can use `.render(JSX, container)` to render *JSX* inside the *container*

```javascript
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(<h1>Hello world</h1>, document.getElementById('app'));
```

## Advenced JSX

### class vs className

`class` is a reserved word in javascript. Therefore I **can't** use it in a JSX element. I have to use instead `className` that will be rendered as `class`. EG :

```jsx
<div className="myDiv">hey!</div>
```

will be rendered as :

```jsx
<div class="myDiv">hey!</div>
```

### Self Closing Tags

In HTML, the end-slash is optional :

`<br>` works as good as `<br />`

in JSX, the **end-slash is required** :

```jsx
// Works :
<br />
// Doesn't works :
<br>
```

### include Javascript inside JSX

Use curly braces marker `{}` to inject javascript inside JSX. Eg :

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

**ACHTUNG** lowercase for HTML, such as `onmouseover`, camelCase for JSX, such as `onMouseOver`.

The event Listener value should be a function.

### Conditionals

`if` statement is not allowed in JSX. There's some workaround :

#### Conditional outside

Set the conditional **outside** of the JSX element :

```javascript
if (condition == true) {
  var myJSX = (
    <h1>
      Hey it is True!
    </h1>
  );
} else {
  var myJSX = (
    <h1>
      Ooooh it is False.
    </h1>
  );
}
```

#### Ternary Operator

It's the same as an is/else statement, but in one line :

```javascript
userIsYoungerThan21 ? serveGrapeJuice() : serveWine();```
```

They can be chained :

```javascript
var k = a ? (b ? (c ? d : e) : (d ? e : f)) : f ? (g ? h : i) : j;
```

And this **can** be used inside a JSX element.

#### && operator

Can be used to make somehting, or nothing :

```javascript
var doesHeLikeIt = (
  { age > 15 && <p>Brussels Sprouts</p>}
);

// is interpreted as :
// if (age is superior as 15) is true, and (<p>...</p>) is true (it's always true...), then use it !
```

### .map in JSX

If I want to creat a list of JSX elements, I can use an Array or *not* :

```javascript
// This is fine
var listNormal = (
  <ui>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ui>
);

//This is also fine!
var list = [
      <li>1</li>
      <li>2</li>
      <li>3</li>
];
var listWithArray = <ul>{list}</ul>;
```

.map iterate through an array and can return a new array :

```javascript
var array = ['1', '2', '3'];

var numbersInList = array.map(function(num){
  return <li>{num}</li>
});
```

### Keys

A ```key```is a JSX attribute, used like this :

```jsx
<ul>
  <li key="li-1">Example1</li>
  <li key="li-2">Example2</li>
  <li key="li-3">Example3</li>
</ul>
```

React use `keys` internally to keep track of lists and which items have been added, changed or removed. In some case it's important to use them to help React behave right. Especially, I must use `keys` if :

- The list-items have *memory* from one render to the next, like a check-box list.
- The list order mught be shuffled.

There is several way to assign a key. One would be :

```javascript
var people = ['toi', 'moi', 'lui'];

var peopleArray = people.map(function(person, i){
  return <li key={'person_'+i}>{person}</li>
});
```

### React.createElement()

React compiler transform JSX into the method .creatElement(). If I want, I can use directly this method to create elements :

```javascript
React.createElement(
"h1",
null,
"Hello, World")
```

# React Components

## Intro to Components

### Component Class

```javascript
var MyComponentClass = React.createClass({
  render: function () {
    return <h1>Hello world</h1>
  }
});
```

- `var MyComponentClass = React.createClass()` create a new component class and store it in a new variable. Use *UpperCamelCase* for the class name.

- `.creatClass()` takes an *object* as argument. This object **must** include at least a *render function* : a property whose name is `render` and whose value is a *function*.

- A render function **must** have a `return` statement (usually a JSX expression).

### Component Instance

Create a component instance by writing a JSX element with the component class name as tag :

```jsx
<MyComponentClass />
```

JSX elements can be either HTML-like, or component instance. To make the difference, JSX use the first letter capitalisation of the tag :

`<myDiv />` is an **HTML-like** JSX element
`<MyDiv />` is an **component** instance JSX element

### Render Component

I can render the component instance the same way as JSX expression :

```javascript
ReactDOM.render(<MyComponentClass />, document.getElementById('app'));
```

##Component and JSX together

###Use Variable attribute

I can create an object and use it's properties inside a JXS :

```javascript
var itIsMe = {
  name: 'Camille',
  trasportation: 'bike',
  favoriteReciepe: 'suchis'
}

var myJSX = ( // I also could create a Class here
  <div>
    <h1>{itIsMe.name}</h1>
    <p>i like {itIsMe.transportation} and {itIsMe.favoriteReciepe}</p>
  </div>
  )
```

### Put logic in a render founction

In a render function, I can use logical operators, comparaison, statements... 
The logic should be before the return statelment :

```javascript
var numbers = { 1: "un",
                2: "deux"
                3: "trois"}

render: function(){
  var num = numbers[0];
  return <div>{num]</div>;
```

```javascript
render: function(){
  var plan;
  if (fiftyFifty){
    plan = "up";
  }else{
    plan = "down";
  }
  return <div>{plan]</div>;
```

I should be carefull to put the statement **inside** the function, and **before** the render

### this

[Reminder about *this*](http://www.digital-web.com/articles/scope_in_javascript/)

```javascript
var MyName = React.createClass({
  name: "camille",
  render: function () {
    return <h1>My name is {this.name}.</h1>;
  }
});
```

## Component rendering component

Each component is usually small on it's own. I combine them to form comlpex ecosystems of informations

React is spacial in the ways in which components interact.

### Basic Rendering

```javascript
var OMG = React.createClass({
  render: function () {
    return <h1>Whooaa!</h1>;
  }
});

var Crazy = React.createClass({
  render: function () {
    return <OMG />;
  }
});
```

### Imports

[Why Module System is important](http://eloquentjavascript.net/10_modules.html)

[React's Module System comes from Node.js](http://fredkschott.com/post/2014/06/require-and-the-module-system/)

#### Import a whole file

Start the path with `.` or `/` to let know javascript where giving a path.

```javascript
var MyModule = require('/path-to-module.js') // .js is faculative
```

#### Import a Class

I usually don't want to import a whole file, but just the Component Class.

I must first export the required module in his file.
[module.exports in Node.js](https://www.sitepoint.com/understanding-module-exports-exports-node-js/)

```javascript
var OMG = React.createClass({
  render: function () {
    return <h1>Whooaa!</h1>;
  }
});

module.exports = OMG;
```
