# React Notes

### Component Declarations
* **Functional Stateless Components:** These components are functions which get an input and return an output. The input is the props object. The output is a component instance. While similar to to an ES6 class component, functional stateless components are functions and they have no internal state. Their state cannot be accessed with `this.state` because there is no `this` object. In addition, they have no lifecycle methods.
* **ES6 Components:** In the class definition they extend from the React component. The `extend` hooks all the lifecycle methods (who are available via the React component API), you can store and manipulate state in ES^ class components.

A rule of thumb for using functional stateless components over ES6 class components is when you don't need internal component state or component lifecycle methods. The `App` component in a React app uses internal state which is why it's a ES6 class component

ex:
class Component
```JSX
function Search({value, onChange, children }) {
    return(
        <form>
            {children} <input
                type="text"
                value={value}
                onChange={onChange}
            />
        </form>
    );
}
```

functional stateless
```JSX
const Search = ({value, onChange, children}) =>
    <form>
        {children} <input
            type="text"
            value={value}
            onChange={onChange}
        />
    </form>
```
### Lifecycle Methods
There are many lifecycle methods, two commonly used ones are `render()` and `constructor()`.
* `constructor()` is only called when a instance of the component is created and inserted in the DOM. The component gets instantiated, this process is called "mounting of the component".
* `render()` method is called during the mount process too, but it's also called when the component updates. Each time the state or props of a component are changed, the `render()` method is called.
* The mounting of a component has two more lifecycle methos: `componentWillMount()` and `componentDidMount()`. The constructor is called first, `componentWillMount()` gets called before the `render()` and `componentDidMount()` is called after the `render()`method.
* Overall the mounting process has 4 lifecycle methods. They are invoked in the following order:
    * `constructor()`
    * `componentWillMount()`
    * `render()`
    * `componentWillMount()`
* The update lifecycle of a component is what happens when the state or prop changes. It has 5 lifecycle methods, which are in the following order:
    * `componentWillReceiveProps()`
    * `shouldComponentUpdate()`
    * `componentWillUpdate()`
    * `render()`
    * `componentDidUpdate()`
* Lastly is the unmounting lifecycle. it only has one lifecycle method: `componentWillUnmount()`
These lifecycle methods can be used for specific use cases such as:
* `constructor(props)` - It's called when the component gets initialized. You can set an initial state and bind useful class methods during that lifecycle method.
* `componentWillMount()` - It's called before the `render()` lifecycle method. It can be used to set internal component state, as it will not trigger a second rendering of the component. It's recommended to use the `constructor()` method to set the initial state.
* `render()` - The lifecycle method is mandatory and returns the elements as an output of the component. The method should be pure and therefore shouldn't modify the component state. it gets an input as a prop and state and returns an element.
* `componentDidMount()` - It's called *only* once when the component is mounted. It's the perfect time to do an async request to fetch data from an API. The fetched data would be stored in the internal component state to display it in the `render()` lifecycle method.
* `componentWillReceiveProps(nextProps)` - This lifecycle method is called during an update lifecycle. As input you get the next props. You can diff the next props with the previous props (`this.props`) to apply a difference behavior based on the diff. Additionally you can set state based on the next props.
* `shouldComponentUpdate(nextProps, nextState)` - It's always called when the component updates due to state or props changes. It's used in React project as a performance optimization. Depending on the boolean that is returned from this lifecycle method, the component and all its children will render or not render on an update lifecycle.
* `componentWillUpdate(nextProps, nextState)` - This lifecycle method is immediately invoked before the `render()` method. The next props and next state are already ready to be used. The method can be used as a last opportunity to perform preparations before the render method gets executed. `setState()` can no longer be triggered. `componentWillReceiveProps()` must be used if computing state based on the next props.
* `componentDidUpdate(prevProps, prevState)` - This lifecycle method is immediately invoked after the `render()` method. It is used to perform DOM operations or to perform further async requests
* `componentWillUnmount()` - This is called before a component is destroyed, this lifecycle method is used to perform any clean up tasks.

### ES6 Import and Export

`import` and `export` work much the same as most other programming languages, it helps to share code across multiple files and code can be distributed across multiple files to keep it resuable and maintainable. It also emphasises code encapsulation, as in not every functionality needs to get exported from a file. Some functionalities may only be used in the file where they are defined. Only the exported functionalities are avaibale to be reused somewhere else (kinda like a public API).

ex
```Javascript
const firstname = 'Bob';
const lastname = 'Vitto';

export{firstname, lastname};

//This can be imported into another file
import {firstname, lastname} from './exportFile.js'
console.log(firstname);
//Output: Bob

//Can also import all exported variables from another file as one object
import * as person from './exportFile.js';
console.log(person.firstname);
//Output: Bob

// Imports can have an alias. It can happen importing functionalities from multiple files that have the same named export.
import {firstname as foo} from './exportFile.js';
console.log(foo);
// Output: Bob
```

There is also the `default` statement which is used for:
* To export and import a single functionality
* To highlight the main functionality of the exported API of a module
* to have a fallback import functionality

ex:
```Javascript
const bob = {
    firstname: "Bob",
    lastname: "Vitto",
};
export default bob;

//import
import developer from './exportFile.js';
console.log(developer);
// Output: {firstname: 'Bob', lastname: 'Vitto'}
```

The import name can differ from the exported default name. It can be used in conjunction with the named export and import statements.

```Javascript
const firstname = "Bob";
const lastname = "Vitto";

const person = {
    firstname,
    lastname,
};

export {
    firstname,
    lastname,
};

export default person;

//import

import developer, {firstname, lastname} from 'exportFile.js';
```
