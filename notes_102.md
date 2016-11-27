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