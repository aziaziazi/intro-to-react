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

## **props.** : Components Interacting 

A **`props`** is an object that hold information about a component. Every components got one. I can use `props` to pass information from a component to another one.

**prop** : Each piece of info passed-in
**props** : All the pieces of infos passed-in OR the objects that holds those infos.

### Basics

#### Pass

I can *pass information* to a React component by giving that component an *attribute*. For a string, use *Quotation marks*. Use *curly brackets* for anything else : object, array, integer...

```javascript
// Pass a String
<MyComponent foo="bar"/>

// Pass an Integer
<MyComponent foo={1234}/>

// Pass an Array
<MyComponent foo={["this", "is", "not", "super", "secret"]}/>
```

I also can pass several arguments :

```javascript
<MyComponent name="Cam" eyes={2} girlfriend="Kim"/>
```

#### Access

To access the whole objects.props : `this.props`. If I want a string : `JSON.stringify(this.props)`.

To access a specific value : `this.props.name`.

```javascript
render: function () {
    var data = this.props.data;
    return <h1>Hi there, {this.props.other-data}!</h1>;
}
```

### From Component to Component

1. In Greeting.js, export Greeting Class with `module.exports`
2. In App.js, import Greeting Class with `require()`
3. In App.js, create and *instance* of Greeting and give it an *attribute* with `<Greeting attribue="value"/>`
4. In Greeting.js, use that *prop* (the attribute) with `{this.prop.attribute}`

```javascript
// Greeting.js
var Greeting = React.createClass({
  render: function () {
    return <h2>Hi there, {this.props.name}!</h2>; // prop use to display it's value
});

module.exports = Greeting;

// App.js
var Greeting = require('./Greeting');

var App = React.createClass({
  render: function () {
    return (
        <h1>This is my App!</h1>
        <Greeting name="Camille"/>
      );
});
```

I also can use props to make decisions :

```javascript
var Welcome = React.createClass({
  render: function () {
    if (this.props.language == 'Deutsch') { // prop use as a condition
      return (
        <h2>
          Hallo ! i spreache nicht deutsch, enschuldigung
        </h2>
      );
    } else {
      return (
        <h2>
          Wwelcoooome!!
        </h2>
      );
    }
  }
});
```

### Passing an event Handler

I also can pass functions as *props* ! And thoses function can be events handling. Here's how to bind the function to the event :

1. In Talker.js, Create the function handling the event. The convention for naming is : "handle" + event type >> `handleClick`
2. In Talker.js, pass the function handling the event to the instance created. Use `this.` to refer to it. The conviention for naming the prop is : "on" + event type >> `onClick`
3. In Button.js, bind the prop to the html event handler

```javascript
// Talker.js
var Talker = React.createClass({
  handleClick: function () {
    // Here any processing I want
    alert(blaaah);
  },
  
  render: function () {
    return <Button onClick={this.handleClick}/>;
  }
});

// Button.js
var Button = React.createClass({
  render: function () {
    return <button onClick={this.props.onClick}>ClickMe</button>;
  }
});
```

### this.props.**children**

When I declare a component, I can use self closing tag OR not :

```javascript
<MyComponent />
// is equal to :
<MyComponent>
</MyComponent>
```

`this.props.children` will return everything in beetween a component's opening and closing tag. If there's several elements, an array is returned.

```javascript
// App.js
var App = React.createClass({
  render: function () {
    return (
        <List type='Living Cat Musician'> // Instance creation
          <li>Nora the Piano Cat</li>     // Children of thi instance
        </List>
    );
  }
});

// List.js
var List = React.createClass({
  render: function () {
    var titleText = 'Favorite ' + this.props.type;
  }
    return (
      <div>
        <h1>{titleText}</h1>
        <ul>{this.props.children}</ul>    // Call to the children >
      </div>
    );
  }
});
```

### getDefaultProps

I can set default props to fallback. To that, I have to make a function `getDefaultProps` and return inside it an object with the default properties I want : 

```javascript
var Top = React.createClass({
  getDefaultProps: function(){
    return {title: "default title yeah!",
            text : "default text ooooh!"};
  },
  
  render: function () {
    return (
      <h1>{this.props.title}</h1>
      <p>{this.props.text}</p>
    );
  }
});

ReactDOM.render(
  <Top />, // If 'title' or 'text' are passed, default values will be overided
  document.getElementById('app')
);
```

