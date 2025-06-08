# React

- [General Notes](#general-notes)
- [Prop Forwarding](#prop-forwarding)
- [Refs](#refs)
- [React Context API](#react-context-api)
- [useReducer](#usereducer)

---

## General Notes

- When wirting React you are writing **_Declarative_** code. This is describing what the state of the app should look like and React will figure out how to get to that state and preform the necessary steps. This is opposed to **_Imperative_** code where you describe the steps to get to the desired outcome.
- The `useEffect` hook in React allows you to perform side effects in functional components. Side effects can include:
  - Fetching data
  - Directly updating the DOM
  - Setting up subscriptions
  - Timers or intervals

---

## Prop Forwarding

**_Prop forwarding_** is a technique where you pass all props received by a component directly to its child component using the spread operator (`...`). It's particularly useful when you're creating wrapper components and want to pass through all the standard HTML attributes without listing them individually.

```javascript
// Instead of doing this (listing each prop individually):
const Button = ({ className, disabled, type, onClick, children }) => {
  return (
    <button
      className={className}
      disabled={disabled}
      type={type}
      onClick={onClick}
    >
      {children}
    </button>
  );
};

// You can forward props using the spread operator like this:
const Button = (props) => {
  return <button {...props} />;
};
```

---

## Refs

**_Refs_** in React provide a way to directly access DOM elements or React components that have been rendered. While React typically handles DOM manipulation through its virtual DOM, there are cases where you need direct access - like managing focus, text selection, or integrating with third-party libraries.

```javascript
import { useRef } from "react";

function TextInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

Common use cases for refs include:

1. Managing focus, text selection, or media playback
2. Triggering imperative animations
3. Integrating with third-party DOM libraries
4. Accessing input values

However, you should use refs sparingly. In most cases, you should rely on React's data flow and state management instead. Refs are an "escape hatch" for cases where you truly need direct DOM access.

---

## React Context API

The **_React Context API_** is used to avoid having to use prop drilling to pass state around an app.

Note: _Context is not optimized for high-frequency updates and might not be suitable for certain performance-critical scenarios._

[_additional details_](./context-details.md)

_Additional Note:_ There is also a common pattern where you create a custom hook in the `context/UserContext.jsx` file that helps with error prevention, future proofing and other things. See the Claude chat titled [React Router State Preservation Strategies](https://claude.ai/chat/d19ade90-4a3e-477a-bf07-cfe5ba97213f) for additional details.

Here's a simple reference breakdown of the three key parts of React Context, with code examples:

1. **Context Container**
   - Created using createContext()
   - Usually named with PascalCase (like MyContext)
   - Holds the structure of your shared data

```javascript
export const UserContext = createContext({
  username: "",
  updateUsername: () => {},
});
```

2. **Built-in Provider**
   - Automatically created as .Provider on your context
   - Always named MyContext.Provider
   - Used to wrap components that need access to the context

```javascript
<UserContext.Provider value={someValue}>{children}</UserContext.Provider>
```

3. **Custom Provider Component**
   - Your wrapper component that manages the state/logic
   - Usually named like MyContextProvider
   - Uses the built-in Provider internally but adds your state management

```javascript
export default function UserContextProvider({ children }) {
  const [username, setUsername] = useState("");

  function updateUsername(newName) {
    setUsername(newName);
  }

  return (
    <UserContext.Provider
      value={{
        username: username,
        updateUsername: updateUsername,
      }}
    >
      {children}
    </UserContext.Provider>
  );
}
```

This separation helps keep your context organized: the container defines what you're sharing, the built-in Provider handles the React magic of making it available, and your custom Provider component manages how the data actually behaves.

### Full Context Example

#### Set up

```javascript
// context/UserContext.jsx
import { createContext, useState } from "react";

export const UserContext = createContext({
  username: "",
  updateUsername: () => {},
});

export default function UserContextProvider({ children }) {
  const [username, setUsername] = useState("");

  function updateUsername(newName) {
    setUsername(newName);
  }

  const ctxValue = {
    username: username,
    updateUsername: updateUsername,
  };

  return (
    <UserContext.Provider value={ctxValue}>{children}</UserContext.Provider>
  );
}
```

#### Wrap Context around components that will consume it

```javascript
// App.jsx
import UserContextProvider from "./context/UserContext.jsx";

export default function App() {
  return (
    <UserContextProvider>
      <Header />
      <Body />
      <Footer />
    </UserContextProvider>
  );
}
```

#### Consume / Use Context

```javascript
// Body.jsx
import { useContext } from "react";
import { UserContext } from "../context/UserContext";

export default function Body() {
  const userCtx = useContext(UserContext);
  const { username, updateUsername } = userCtx;

  return (
    <>
      <div>Username: {username}</div>
      <button onClick={() => updateUsername("Bob")}>
        Update User Name to Bob
      </button>
    </>
  );
}
```

---

## useReducer

`useReducer` and `useState` are equivalent. You can always convert between them. _(see [React Docs](https://react.dev/learn/extracting-state-logic-into-a-reducer#comparing-usestate-and-usereducer))_

Here's a simple reference breakdown of the key parts of useReducer:

1. **Reducer Function**
   - Takes current state and action as parameters
   - Returns new state based on action type
   - Pure function: same inputs always return same output

```javascript
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
}
```

2. **Initial State**
   - Defines starting value(s) for your state
   - Can be a simple value or complex object

```javascript
const initialState = { count: 0 };
```

3. **useReducer Hook**
   - Returns current state and dispatch function
   - Dispatch sends actions to the reducer
   - Takes reducer function and initial state as parameters

```javascript
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>-</button>
    </div>
  );
}
```

This pattern is especially useful for complex state logic: the reducer function centralizes state updates, dispatch actions describe what happened, and the state holds your data.

### Full useReducer Example

```javascript
// CounterWithReducer.jsx
import { useReducer } from "react";

// Reducer function - determines how state updates in response to actions
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    case "RESET":
      return { count: 0 };
    case "ADD":
      return { count: state.count + action.payload };
    default:
      return state;
  }
}

// Initial state - starting value for our counter
const initialState = { count: 0 };

export default function CounterWithReducer() {
  // Set up useReducer - returns current state and a dispatch function
  const [state, dispatch] = useReducer(reducer, initialState);

  // Event handlers - use dispatch to send actions to the reducer
  const handleIncrement = () => {
    dispatch({ type: "INCREMENT" });
  };

  const handleDecrement = () => {
    dispatch({ type: "DECREMENT" });
  };

  const handleReset = () => {
    dispatch({ type: "RESET" });
  };

  const handleAddFive = () => {
    dispatch({ type: "ADD", payload: 5 });
  };

  return (
    <div className="counter-container">
      <h1>Counter with useReducer</h1>

      <div className="counter-display">
        <p>Count: {state.count}</p>
      </div>

      <div className="counter-controls">
        <button onClick={handleDecrement}>-</button>
        <button onClick={handleIncrement}>+</button>
        <button onClick={handleReset}>Reset</button>
        <button onClick={handleAddFive}>Add 5</button>
      </div>

      {/* Showing different ways to dispatch */}
      <div className="counter-advanced">
        <h3>Alternative Dispatch Methods:</h3>

        {/* Direct dispatch in onClick */}
        <button onClick={() => dispatch({ type: "INCREMENT" })}>
          Increment (Inline)
        </button>

        {/* Dispatch with payload directly in onClick */}
        <button onClick={() => dispatch({ type: "ADD", payload: 10 })}>
          Add 10
        </button>
      </div>

      {/* Conditional rendering based on state */}
      {state.count > 10 && (
        <div className="counter-message">
          <p>You've reached a high count!</p>
        </div>
      )}
    </div>
  );
}
```
