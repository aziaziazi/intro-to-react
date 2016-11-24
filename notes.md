TODO: Add summary

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

## props. : Components Interacting 

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

### this.props.children

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
          <li>Nora the Piano Cat</li>     // Children of this instance
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

# state. : Store dynamic informations

*Dynamic information* is an information that can change. Eg : a component displaying a game's score. There are two ways for a component to get dynamic information : `props`to and `state`. 

## InitialState

To make a component have a state, I have to write a `getInitialState` funtcion :

```javascript
var Example = React.createClass({
  getInitialState: function () {
    return { mood: 'decent' };
  },

  render: function () {
    return <div></div>;
  }
});

<Example />
```

## Read state

Use that expression to read a component state :

```javascript
{this.state.name-of-property}
```

## Update state

Use `this.setState` to change the state of a component. The most cummon way to call `this.setState` is to call a custom function that wraps it : 

```javascript
  addAMuffin: function () {
    this.setState({ muffinsNb: muffinsNb += 1 });
  },
```

Also, any time I call `this.setState`, `render()` is called as soon as the state has changed. Therefore I **can't** call `this.state` inside a render function, otherwise I would creat an infinite loop.

```javascript
var Toggle = React.createClass({
  
  getInitialState: function(){
    return { color: green }
  },
  
  changeColor: function(){
    var newColor = this.state.color == green ? yellow : green;
    this.setState({ color: newColor });
  },
  
  render: function () {
    return (
      <div style={{background:this.state.color}}>
        <button onClick={this.changeColor}>
          Change color
        </button>
      </div>
    );
  }
});
```

# Programming pattern : Stateful and Stateless components

I should use `props` to store information that can only be changed by a different component.
I should use `state` to store infomration that can be change by the component itself.

A **Stateful** component is a component that as a `getInitialState` function.
A **Stateless** component is a component that doesn't.

## Stateless Components Inherit From Stateful Components

Passing info from a stateful Parent to a stateless Child:

```javascript
//Child.js : Use of props because it doesn't change it itself.
var Child = React.createClass({
  render: function(){
    return <h1>Hey, my name is {this.props.name}!</h1>;
  }
})

//Parent.js : Create a state that can change, and pass it to Child's prop.
var Parent = React.createClass({
  getInitialState: function(){
    return { name: 'Frarthur' }
  },
  
  render: function(){
    return <Child name={this.state.name}/>;
  }
})
```

## Child Components Update Their Parents' state

### Parent pass a state changing function to Child

1. Create getInitialState of what I want to change : `name`
2. Render a Child and pass him the `name` state as a prop
3. Define a `changeName` function to change the state
4. Pass `changeName` to the Child by a prop

```javascript
//Parent
var Parent = React.createClass({
  getInitialState: function () {
    return { name: 'Frarthur' };          // 1
  },
  
  changeName: function(newName){
    this.setState({ name: newName });     // 3
  },

  render: function () {
    return (
      <Child
        name={this.state.name}            // 2
        onChange={this.changeName}/>      // 4
    );
  }
});
```

### Child update Parent state

1. Create an event listener in the rendered JSX.
...If the Parent's function doesn't need an argument, I can directly pass it here! Hoever I want to pass the dropdown menu's value, so :
2. Call an handling function, passing the event `(e)` and
3. Create the `handlingFunction(e)` that  extract the value I need and
4. pass it to the parent's function !

```javascript
//Child
var Child = React.createClass({
  handleChange:function(e){               // 3
    var name = e.target.value;
    
    this.props.onChange(name)           // 4
  },
  
  render: function () {
    return (
      <div>
        <h1> Hey my name is {this.props.name}! </h1>
                                          // 1 //2
        <select onChange={this.handleChange} id="great-names">
          <option value="Frarthur"> Frarthur </option>
          <option value="Gromulus"> Gromulus </option>
          <option value="Thinkpiece"> Thinkpiece </option>
        </select>
      </div>
    );
  }
```

[Auto-binding](https://facebook.github.io/react/blog/2013/07/02/react-v0-4-autobind-by-default.html);

## Child Components Update Their Siblings' props

I want every component have only one purpose. The last Child exemple has two : diplaying a `prop` and change it. I should split it in two component : *Child* to change the `prop` and *Sibling* to display it. There's still the 
*Parent* component that store the value :

```javascript
// Parent
var Parent = React.createClass({
  getInitialState: function () {
    return { name: 'Frarthur' };
  },
  
  changeName: function (newName) {
    this.setState({
      name: newName
    });
  },

  render: function () {
    return (
      <div>
        <Child onChange={this.changeName} />
        <Sibling name={this.state.name} />
      </div>
    );
  }
});

// Child
var Child = React.createClass({
  handleChange: function (e) {
    var name = e.target.value;
    this.props.onChange(name);
  },
  
  render: function () {
    return (
      <div>
        <select 
          id="great-names" 
          onChange={this.handleChange}>
          
          <option value="Frarthur">Frarthur</option>
          <option value="Gromulus">Gromulus</option>
          <option value="Thinkpiece">Thinkpiece</option>
        </select>
      </div>
    );
  }
});

// Sibling
var Sibling = React.createClass({
  render: function () {
    var name = this.props.name;
    return (
      <div>
        <h1>Hey, my name is {name}!</h1>
        <h2>Dont you think {name} is the prettiest name ever?</h2>
        <h2>Sure am glad that my parents picked {name}!</h2>
      </div>
    );
  }
});

```

For a reminder of each step : [Codecademy review](https://www.codecademy.com/courses/react-102/lessons/child-updates-sibling/exercises/stateless-inherit-stateful-recap)

# Advenced React

## Inline Styles

There are many ways to use styles in React. Here I'll learn *inline* styles, which is a style written as an *attribute*:

```javascript
<h1 style={{ color: 'Red'}} >Hello World</h1>
```

Here I have to use double curly braces :
- one for injecting Javascript into JSX
- one for creating a javascript object literal

### Create a style object

however if I have many styles it's often nicer to store the style object in a variable  and pass it to the JSX. In this case, only one curly braces is suffisant :

```javascript
var styles = {
  background: 'lightblue',
  color:      'darkred'
};

var styleMe = <h1 style={styles}>Please style me</h1>;
```

### Names & Values writting

In javascipt, style names are written in hyphenated-lowercase like `margin-top`. In React, I should use camelCase:

```javascript
var styles = { 
  marginTop:        '20px',
  backgroundColor:  'green'
};
```

In javascirpt, style value are almost always `"strings"`, even for numbers with units. In React it's the same but I also can write *numbers* and it will assume the unit `px`:

```javascript
{ fontSize : 30 }
// is equal to
{ fontSize: "30px" }
```

### Re-use

Keep the styles in a separate javascript file to reuse them. Export via `module.export` and import with `require()` :

```javascript
// colorPalette
var lightBlue = 'rgb(223, 227, 238)';
var grey      = 'rgb(247, 247, 247)';

module.exports = {
  lightBlue: lightBlue,
  grey: grey
};

// Component
var styles = require('./colorPalette');

var divStyle = {
  backgroundColor:  styles.grey,
  color:            styles.lightBlue
}

var Component = React.createClass({
  render: function() {
    return <div style={ divStyle } />
  }
});
```

## Seprataing Container Components From Presentational Components

I should **always separate** the components that handle the presentation and the components that does the calculs.

- Presentations Components contains the **JSX**. They contain only one function : the `render` function (see next chapter)
- Container Components contains **everythink but no JSX**

To have my folders neat, I can create one folder for (presentation) **Components** and one folder **Containers** (Components).

```javascript
// Components/GuineaPigs.js
var GuineaPigs = React.createClass({
  render: function () {
    var src = this.props.src;
    return (
      <div>
        <h1>Cute Guinea Pigs</h1>
        <img src={src} />
      </div>
    );
  }
});
```

```javascript
// Containers/GuineaPigsComponent.js
var GUINEAPATHS = [
  'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-guineapig-1.jpg',
  'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-guineapig-2.jpg',
  'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-guineapig-3.jpg'
];

var GuineaPigsContainer = React.createClass({
  getInitialState: function () {
    return { currentGP: 0 };
  },

  nextGP: function () {
    var current = this.state.currentGP;
    var next = ++current % GUINEAPATHS.length;
    this.setState({ currentGP: next });
  },

  interval: null,

  componentDidMount: function () {
    this.interval = setInterval(this.nextGP, 200);
  },

  componentWillUnmount: function () {
    clearInterval(this.interval);
  },

  render: function () {
    var src = GUINEAPATHS[this.state.currentGP];
    return (
      <GuineaPigs src={src} />
    );
  }
});
```

## Stateless Functional Components

When I separate the *container component* from the* presentational component*, the presentational component always end up having only one render function and no other properties.

There's a different way I can write them, using a single javascript function. A component like this ,*written as a function* is called **stateless functional component** :

```javascript
// A component class written in the usual way:
var MyComponentClass = React.createClass({
  render: function(){
    return <h1>Hello world</h1>;
  }
});

// The same component class, written as a stateless functional component:
function MyComponentClass () {
  return <h1>Hello world</h1>;
}
```

**To pass a prop**, I just have to pass the `props` object as a paramater :

```javascript
// Normal way to display a prop:
var MyComponentClass = React.createClass({
  render: function () {
    return <h1>{this.props.title}</h1>;
  }
});

// Stateless functional component way to display a prop:
function MyComponentClass (props) {
  return <h1>{props.title}</h1>;
}

// Normal way to display a prop using a variable:
var MyComponentClass = React.createClass({
  render: function () {
    var title = this.props.title;
    return <h1>{title}</h1>;
  }
});

// Stateless functional component way to display a prop using a variable:
function MyComponentClass (props) {
  var title = props.title;
  return <h1>{title}</h1>;
}
```

## PropTypes

I use PropTypes to define... my props' types !  They are usefull for two reasons :

- *Documentation* : they help the reader to *undersand* what the component expect (and the component itself) 
- *Validation* : if `props` are missing or aren't what's expected, a warning will print in the console

Two ways to declare propsTypes :

```javascript
// In a Component Class
var Runner = React.createClass({
  propTypes: {
    message:   React.PropTypes.string.isRequired
  }

  // render : function () {...}
```

```javascript
// In a Stateless Functional Component

// function GuineaPigs (props) {...}

GuineaPigs.propTypes = {
  src: React.PropTypes.string.isRequired
}
```

ACHTUNG : be careful to the capitalization : `propTypes` to declare the object, `.PropTypes` to initialize each props.

The types I can use: `string`, `object`, `bool`, `number`, `func` or `array`.
`isRequire` can be added.

## Forms

In React form, I want the server to know every changes, new character or deletion. For doing that, I keep track of my form through a state :

1. Give `<input />` an `onChange` attribute with value equal to `{this.handleUserInput}`
2. Create `handleUserInput` function that `setState` the `userInput` equal to the event targeted value.
3. Create the `getInitialState` function for `userInput`
4. Use `{this.state.userInput}` anywhere I want in the component !
 

```javascript
var Input = React.createClass({
  getInitialState: function () {          // 3
    return { userInput: '' }
  },
  
  handleUserInput: function (e) {         // 2
    this.setState({
      userInput: e.target.value
    })
  },
  
  render: function () {
    return (
      <div>
        <input
          onChange={this.handleUserInput} // 1
          value={this.state.userInput}    //4
          type="text" />
        <h1>{this.state.userInput}</h1>   // 4
      </div>
    );
  }
});
```

Since I have the value stored as state, I can send it to the server anytime I want and I don't need `<form>`. I still can use it if I want a submit button to differenciant in-progress form and finished form.

**Controlled Components** doesn't store any state by themselfes and I can control their states through `props`.

**Uncontrolled Components** keep tracks of their states by themselfes. Eg : `<input />` knows what's his state and i can query it anytime. In React, when i give an `<input />` a `value` attribute, it becomes *controlled* and stops using his internal storage. This is a more *React* way os doing thinks, but there's workarounds if I want to work with uncontrolled components. [Official documentation here](https://facebook.github.io/react/docs/forms.html)

# Lifecycle Methods

*Lifecycle Methods* are methods that get called automatically at certains moments in a component's life. There's three lyfecycle methods categories :

- mounting
- updating
- unmounting

## Mounting Lifecycle Methods

A component *mount* when it renders for the first time. When it mount, it calls three mounting lifecycle methods in this order :

- `componentWillMount`
..Get called the first time the component is mounted, before `render` started.
- `render`
..Is the well known and required function in a component class
- `componentDidMount`
..Get called the first time the component is mounted, after `render` finished.
..It's here that I usually want to **make AJAX call**, **connect to web APIs** or **JavaScript frameworks**.

I call them like that : 

```javascript
var Flashy = React.createClass({
  componentWillMount: function () {
    alert('FOR THE FIRST TIME EVER...  FLASHY!!!!');
  },

  componentDidMount: function () {
    alert('JUST SAW THE DEBUT OF...  FLASHY!!!!!!!');
  },
  
  render: function () {
    alert('Flashy is rendering!');
    
    return (
      <h1 style={{ color: this.props.color }}>
        OOH LA LA LOOK AT ME I AM THE FLASHIEST
      </h1>
    );
  }
});
```

At first munting of the component ( `ReactDOM.render()` called for the first time), here's what happening :

1. componentWillMount > `alert('FOR THE FIRST TIME EVER...  FLASHY!!!!)`
2. render > `alert('Flashy is rendering!')` + return() and *Rendering*
3. componentDidMount > `alert('JUST SAW THE DEBUT OF...  FLASHY!!!!!!!')`

At nexts renderings :

1. render > `alert('Flashy is rendering!')` + return() and *Rendering*

## Updating Lifecycle Methods

A component *update* everytimes that it renders, starting with the second render (the first time, it *mount*). When it updates, it calls the mounting lifecycle methods in this order :

- `componentWillReceiveProps`
..Get called only if the component will receive `props`. It automatically get passed one argument : `nextProps`. It's an object that is a preview of the upcoming `props`.
- `shouldComponentUpdate(nextProps, nextState)`
..It should return either:..
..- If `true` the process continue as normal.
..- If `false` the process stops here and none of the below lifecycle methods will be called, including `render`
..It automatically receive two arguments : `nextProps` and `nextState`. It's typical to compare them to `this.props` and `this.state` to decide what to do. (see exemple bellow).
- `componentWillUpdate(nextProps, nextState)`
..I can't call `this.setState` here.
..Here, I usually want to do non-React things, like checking the `window` size or on interacting with an API.
- `render`
- `componentDidUpdate(nextProps, nextState)`
..Similar to `componentWill Update` but get called after `render`: no `this.setState` and good for non-React and APIs

```javascript
var Example = React.createClass({
  getInitialState: function () {
    return { subtext: 'Put me in an <h2> please.' };
  },
  
  componentWillReceiveProps: function (nextProps) {
      alert("Check out the new props.text that I'm about to get:  "
      + nextProps.text);
    },

  shouldComponentUpdate: function (nextProps, nextState) {
    if ((this.props.text == nextProps.text) && 
      (this.state.subtext == nextState.subtext)) {
      alert("Props and state haven't changed, so I'm not gonna update!");
      return false;
    } else {
      alert("Okay fine I will update.")
      return true;
    }
  },

  componentWillUpdate: function (nextProps, nextState) {
    alert('Component is about to update!  Any second now!');
  },

  componentDidUpdate: function () {
    alert('Component is done rendering!');
  },

  render: function () {
    return (
      <div>
        <h1>{this.props.text}</h1>
        <h2>{this.state.subtext}</h2>
      </div>
    );
  }
});
```

## Unmounting Lifecycle Methods

A component *unmount* when it is removed from the DOM. That happend if the DOM is rendered without the component, if the user navigates to a different website or if he close the browser.

There's only one lifecycle methos : `componentWillUnmount(prevProps, )`. It gets called right before the component is removed from the DOM.

```javascript
// Enthoused.js
// var React = require('react');

var Enthused = React.createClass({
  interval: null,

  componentDidMount: function () {
    this.interval = setInterval(function(){
      this.props.addText('!');
    }.bind(this), 15);
  },

  componentWillUnmount (prevProps, prevState) {
    clearInterval(this.interval);
  },
  
  render: function () {
    return (
      <button onClick={this.props.toggle}>
        Stop!
      </button>
    );
  }
});
// module.exports = Enthused;

// App.js
// var React = require('react');
// var ReactDOM = require('react-dom');
// var Enthused = require('./Enthused');

var App = React.createClass({
  getInitialState: function () {
    return {
      enthused: false,
      text: ''
    };
  },

  toggleEnthusiasm: function () {
    this.setState({
      enthused: !this.state.enthused
    });
  },

  setText: function (text) {
    this.setState({ text: text });
  },

  addText: function (newText) {
    var text = this.state.text + newText;
    this.setState({ text: text });
  },

  handleChange: function (e) {
    this.setText(e.target.value);
  },

  render: function () {
    var button;
    if (this.state.enthused) {
      button = (
        <Enthused 
          toggle={this.toggleEnthusiasm}
          addText={this.addText} />
      );
    } else {
      button = (
        <button 
          onClick={this.toggleEnthusiasm}>
          Add Enthusiasm!
        </button>
      );
    }

    return (
      <div>
        <h1>Auto-Enthusiasm</h1>
        <textarea 
          rows="7"
          cols="40"
          value={this.state.text}
          onChange={this.handleChange}>
        </textarea>
        {button}
        <h2>{this.state.text}</h2>
      </div>
    );
  }
});

// ReactDOM.render(
//  <App />, 
//   document.getElementById('app')
// );
```
