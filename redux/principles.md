# Principles

 - 1) **Immutable state** - everything that changes in your application, including the data and the UI state, is contained in a single object, which we call the **state**. [https://egghead.io/lessons/javascript-redux-the-single-immutable-state-tree#/tab-transcript](https://egghead.io/lessons/javascript-redux-the-single-immutable-state-tree#/tab-transcript)
 - 2) **Actions** - The state is read only. The only way to change the state tree is by dispatching an action, which is a plain JavaScript object describing what changed in the application. Any data that gets into a Redux application gets there by actions. https://egghead.io/lessons/javascript-redux-describing-state-changes-with-actions#/tab-transcript
 - 3) **Reducers** - To describe state mutations, you write a function that takes the **previous state** of the app, the **action** being dispatched, and the **next state** of the app. This function has to be **pure**. This function is called the **reducer**.
 - 4) Here's what a basic **reducer** might look like for a reducer in a increment/decrement app. Notice that there's an initial state (`state=0`) and there's a `default` to handle undefined action types:
```
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```
 - 5) The **store** binds together the three principles of redux: 1) It holds the applications current state. 2) It lets you dispatch actions. 3) When you create it, you need to specify which reducer tells how state is updated with actions.  **getState** gets the current state of the application. At this point, you use
```
import { createStore } from Redux;
const store = createStore(counter);

store.getState();
```

to use the `getStore`.  The most commonly called store method is called **dispatch**. It lets you dispatch actions to change the state of your app.  At this point we use it like this:
```
store.dispatch({ type: 'INCREMENT' });
```
The **subscribe** method lets you register a callback that the redux store will call anytime an action has been dispatch. This is helpful so that you can update the UI of your application. At this point, we can use `subscribe` like this to render the counter to the DOM:
```
const render = () => {
    document.body.innerText = store.getState();
};

store.subscribe(render);
render();

document.addEventListener('click', () => {
    store.dispatch({ type: 'INCREMENT' });
});
```

**QUESTIONS**

 1. For #2 above (**actions**), we say that "Any data that gets into a Redux application gets there by actions". Does this include initial state? The example on shows using actions for state changes.