# Principles

 1. **Immutable state** - everything that changes in your application, including the data and the UI state, is contained in a single object, which we call the **state**. [enter link description here](https://egghead.io/lessons/javascript-redux-the-single-immutable-state-tree#/tab-transcript)
 2. **Actions** - The state is read only. The only way to change the state tree is by dispatching an action, which is a plain JavaScript object describing what changed in the application. Any data that gets into a Redux application gets there by actions. https://egghead.io/lessons/javascript-redux-describing-state-changes-with-actions#/tab-transcript
 3. **Reducers** - To describe state mutations, you write a function that takes the **previous state** of the app, the **action** being dispatched, and the **next state** of the app. This function has to be **pure**. This function is called the **reducer**.



**QUESTIONS**

 1. For #2 above (**actions**), we say that "Any data that gets into a Redux application gets there by actions". Does this include initial state? The example on shows using actions for state changes.