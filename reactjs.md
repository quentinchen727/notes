# The road to react
React is a powerful UI component library, aimed at creating composable UIs, not
a special template language.
CBA: component-based architecture.

React builds large applications with data that changes over time.
generate and render DOM.

Benefits:
1. Simpler apps
    - Declarative over imperative style: how it should be, not what to do.
    - CBA. No domain-specific languages, just `pure Javascript`.
    - Powerful abstractions: normalized/synthesized object and Dom abstraction
      can be rendered on the server.
Under the hood, React uses a virtual DOM to find difference between what's
already in browser and the new view, called DOM diffing or reconciliation of
state and view.
With JQuery, it is DOM manipulation.
With Angular, it is two-way data binding. Angular relies on templates and a DSL
that uses ng directives.
One of the problems with templates is that developers often have to learn yet
another language.
When you're working with template markup and corresponding Js code, you're
working on one functionality. Having two files (.js and .html) is not a
seperation of concerns.  If you use React, you can carry your knowledge to the
next project even if it's not in React.

2. Fast UI.
React updates onoy those parts are absolutely necessary so taht the internal
state(virtual DOM) and the view(real DOM) are the same. Vritual Dom exists only
in Javascript memory. Do unit testing with Jest on the command line.
React is back-end agnostic. You can integrate it with any back end and any
front-end data library.

3. Less code to write.

The react library is split into two packages: React Core and ReactDom.
Various rendering targets, not just DOM.
The spepartion of React Dore form ReactDom makes it easier to share code
between React and React Native libraries(use for native mobile iOS and Android
development)
The spepartion of React Dore form ReactDom makes it easier to share code
between React and React Native libraries(use for native mobile iOS and Android
development).
JSX - a tiny language that lets developers write React UIs more eloquently. You
can transpire JSX into regular JS by using Babel.

### Single-page applications and React
SPA: thick client. Most rendering for UIs happens on the browser.
Angular, backbone, and ember are frameworks for building SPAs.
Other template engines: Underscore, Handlebars, and Mustache.

JSX is a little syntax for writing React objects in Javascript using <> as in
XML/HTML.

React is a view layer. Developers need to pari React with a routing and/or
modeling library.

React is one-way data-binding, while Angular is two-way.

##2. Baby steps with React
Elements are instances of components(also called component classes).
let h1 = React.createElement(Capitalized Cls_name or lowercase html tag string, properties, children..);
ReactDom.render(h1, document.getElementById('content'));

React Developer Tools to install in browsers.

* Creating component classes
class CHILD extends Parent;
class HelloWorld extends React.Component; you must implement render() method,
which must return a single React element.

By convention, the names of varialbes containing React components are
capitalized, which is necessary in JSX,

* Working with properties *
Properties are immutable within their elements.
Properties can be used to:
    - render standard HTML attributes of an element;
    - this.props.Property_name.
Internally, React uses Object.freeze() to make this.props immutable.
Object.isFrozen(obj) == true.
Use the same component but provided with different properties, the elements
rendered by the component can be different.
If there is a match between property and standard attribute, it will be
rendered automatically by React.


## Chapter 3. Introduction to JSX
JSX is a JavaScript extension that provides syntactic sugar for function calls
and object construction, particulary React.createElement(). It may look like a
template engine or HTML, but it isn't. Neither is XHP.

source-to-source compiler, transpiler, or transcompiler is a type of compiler
that takes the source code of a program written in one programming language as
its input and produces the equivalent source code in another programming
language.

All the standard HTML elements in React.DOM object.

* creating elements with JSX
<component key=value></component>

return (
    <div>
    </div>
) // must put a (, if nothing follows return on the same line.

outputing variables in JSX:
<span>Current date is {dateTimeNow}</span> ; // the variable can be properties,
not just locally defined vars, or any JS code.
It can even put React Element in {}.
vs in Js: `current date is ${dateTimeNow}`

Storing data in custom HTML attributes in the DOM is generally considered an
antipattern, bacause getting data from the DOM is slower than getting it from a
virutal/in-memory store.
If you must, you should use the data-Name prefix, otherwise they won't be
rendered by DOM.
Use {...this.props} to pass every property to the child.
<h1 {...this.props}>
The rest is used in a function definition/declaration, and spread is used in
calls and literals.

* Creating React component methods *
Free to write any component methods for your applications, because a React
component is a class.
Invoke a component method drectly from {} and JSX, e.g., {this.getUrl()}
If/else in JSX, unlike in template engines, there's no special syntax for these
conditions in JSX.
Comments: {/* .... */}

Babel is mostly an ES6+/ES2015+ compiler, but it also can convert JSX to
Javascript.

Style attribute:
Css propeties need to be in camelCase, and the value needs to be a JavaScript
object for performance. e.g., {{...}}, one of which is for jsx, and the other is for javascript
object literal.
React and JSX accept any attribute that's a standard html attributes, except
class and for, instead using className and htmlFor.

Use {false} or {true} for boolean attribute values.
some attributes: disabled, required, checked, autofocus, and readOnly are
specific only to form elements.
<input disabled /> // default true
Ternary operators and IIFE are the best ways to implement if/else statements.

## State
A React state is a mutable data store of components-self-contained,
funtionality-centric blocks of UI and logic,
Components are state machines. The state names is an attribute of the
this.state object, e.g., this.state.autocompleteMatches or
this.state.inputFieldValue.

*Setting the initial state*
class MyFancyComponent extends React.Component {
    construct(props) {
        super(props)
        this.state = {...}
    }
} // state is not a class attribute yet.
Notes: set this.state in constructors, so you set it only once.

* Updating states:
setInterval(function, time_interval);
setState() will rerender the DOM by triggering render(), and it only updates the states you pass to
it.
Autobinding means the function created with a fat arrow gets the current value
of this. function (){},bind(this).
setInterval(()=>{
    this.setState({currentTime: (new Date()).toLocalString()})}, 100)
})

* State and Properties
State is mutable while property is immutable.
You pass properties from parent components, whereas you define states in the
component itself, not its parent. So properties determine the view upon
creation, and then they remain static.

* stateless component *
const foo = (props) => {<h1>{...props}</h1>} //React can define it as a
function.

Usually, stateless mean a component created with a function or fat-arrow
syntax.
Keep your stateless components simple: no states and no methods, and don't have
any calls to external methods or functions.
 _
## React component `lifecycle events`
By mounting events, you can inject logic into components.
Categories of events:
- Mounting events: happen when a React element is attached to a DOM node. Only
  once.
- updating events: Happen when a React element is updated as a result of new
  values of its properties or state; many times.
- unmounting events: Happen when a React element is detached from the DOM. Only
  once.
Usage: implement custom logic, fetch data from the backend or integrate with
DOM events or other front-end libraries.
Life cycles:
- Happens when an element is created and lets you set the default properties
  and the initial state.
- Mouinting:
    1. componentWillMount()- Happens before mounting to the DOM
    2. componentDidMount()- Happnes after mounting and rendering
- Updating:
    1. componentWillReceiveProps(nextProps)- Happens when the component is
       about to receive properties;
    2. shouldComponentUpdate(nextProps, nextState)-> bool -Lets you optimize
       the component's redering by determing when to update and when not to,.
    3. componentDidUpdate(prevProps, prevState) - Happnes after teh component
       update.
- Unmounting:
    1. componentWillUnmount function()- lets you unbind and detach any event
       listeners or do other cleanup work before the component is unmounted.

Mounting: constructor()=>componentWillMount()=>render()=>componentDidMount()
Updating properties:
componentWillReceiveProps=>shouldComponentUpdate=>render=>componentWillUpdate=>componentDidUpdate
Updating state:
shouldComponentUpdate=>componentWillUpdate=>render=>componentdidUpdate
updating using: componentWillUpdate=>render=>componentDidUpdate

Avoid using this.forceUpdate().
Pure functions are the cornerstone of `functional programming`, which minimizes
state as much as possible.

To implement an event, define them on a class as methods.
events == event handler == method.
componentDidMount(): a place to put code to integrae with others or xhr
request, cuz at this point the component's element is in the real DOM and u get
access to all its elements.

You can invoke setState() in componentWillMount. render() will get the new
values, if any, and there will be no extra rendering.
The componentDidMount() method o child components is invoked before that of
parent components.

Fetch API: fetch(url).then().then()
Setting your initial values will help you avoid lotsof pain later!

ComponentWillReceiveProps(newProps) is invoked each time there's a
rerendering(of a parent structure or a call), regardless of property-value
changes, but it doesn't mean a real change in the real DOM.
shouldComponentUpdate() is not triggered for the initial render or for
forceUpdate().
componentWillUpdate() will not be called for the initial render.

In componentWillUnmount(), u can invalidate timer, clean up any dom events, or
detachiung events.

## Handling events in React
Define the event handler in JSX:
onClick={function(event){this,...}} // event object is an enhanced version of a
native DOM event object(called SyntheticEvent)
onClick={()=> {...}}
You don't bind the context to the class using bind(this) if:
1. don't refer to this class by using this inside the method
2. use fat arrow (()=>{})

onClick={this.handleSave.bind(this)}

Supported events by React:
mouse, keyboard, clipboard, form, focus, touch, UI, Wheel, Selection, Image,
animation, transition.

Capture phase precedes bubble phase. use onClickCapture to capture events. You
can use this behavior to stop propagation and set priorities between events.

Internally, React keeps track of events attached to higher elements and target
elements in a mapping.

React attaches events to `document`, not to each element, which allows react to
be faster.

getEventListeners(node/$0).remove()

SyntheticEvent: use event.target to get the event target; event.currentTarget
to get the capturing element.
event.target.value to get the txt of an input field.
The synthetic event is nullified once the event hander is done.

Passing event handlers as properties
Dumb and smart components are called presentatinal and container components.
Smart/container components describe how things work without DOM elements, have
states, use higher-order component patterns, and connect to data sources.
States are in container component and dum component is jut a view.

Exchanging data between components
The rule of thumb is to put the event-handling logic in the parent or wrapper
component if you need interaction between child components.

Using the component lifecycle events componentDidMount and componentWillUnmount
to integrate with other libraries or DOM events not supported by React.
window.addEventListener("resize", function);
window.removeEventListener("resize", function);

## Chapter 7. Working with forms in React
In regular HTML, DOM is your storage.
In React, React components must represent the state of the view at any point in
time and not only at initialization time.
The best practice is to keep React's render() as close to the real DOM as
possible.
If you implement an <input> as in HTML, React will always keep render() in sync
with the real DOM, and you won't allow users to change the value.
React need to capture the change and update the state to renrender the view to
reflect the change.
The correct way to work with elements in React: user input -> events -> state
-> view. one-way binding.

Defing a form and its events in React


# React Architecture
## Chapter 12. The webpack build tool
A build tool or a bundler.
Code generators: create-react-app
Webpack knows how to deal with all three types o Javascript module-CommonJS,
AMD and ES6.
