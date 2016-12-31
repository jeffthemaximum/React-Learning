# State and Props

- `state` and `props` help us have our state/logic in one place (containers) and our presentation in another (components).
- `props` are in components
- `state` is in containers.
- `state` is **immutable!**
- We'll see later how to use a special function to change it if we have to, though.

### Example
- From Jingo, in his `HomeContainer.js`, it looks like this:
```
import React from 'react';
import HomeComponent from '../components/HomeComponent';

const HomeContainer = React.createClass({
	getInitialState(){
		return {
			search: true,
			movieTitle: "",
			yourName: "Jason"
		}
	},
	handleUserSubmit(event){
		event.preventDefault();

		console.log(event.target)
		console.log($(event.target).find("input:text").val())
	},
	render(){
		console.log(this.state);
		return(
			<HomeComponent 
				data = {this.state}
				name = {this.state.yourName}
				onUserSubmit = {this.handleUserSubmit}/>
		)
	}
})

export default HomeContainer;
```
- The thing to notice here is `getInitialState()`
- This is `es6` notation to declare a function, like `function getInitialState

----------

##### QUESTION
- Is this defining the function `getInitialState`? Executing it? Both?

----------

- `getInitialState()` stores the values that Jingo has defined in `this.state` as an object with key-value pairs. This is why he can pass around `this.state.yourName` later on.
- The part that will pass the `state` from the `HomeContainer.js` to the `HomeComponent.js` as `props` is this:
```
render(){
        console.log(this.state);
        return(
            <HomeComponent 
                data = {this.state}
                name = {this.state.yourName}
                onUserSubmit = {this.handleUserSubmit}/>
        )
    }
``` 
- Now, `HomeComponent.js` will use those props like this:
```
import React from 'react';

const styles = {
	centering: {
		textAlign: 'center'
	}
}

function HomeComponent(props){
	console.log(props)
	return(
		<div className="container" style={styles.centering}>
			<h1>This Is Your Movie App!</h1>
			<div className="row">
		    <form 
		    	className="col s10 offset-s2 m4 offset-m4"
		    	onSubmit = {props.onUserSubmit}>
		    	<input 
		    		placeholder="Enter Movie or TV Show Title" 
		    		type="text"
		    		className="validate"/>
		    	<input 
		    		type="submit"
		    		hidden/>
		    </form>
		  </div>

		  <h2>The name from our props is {props.name}</h2>
		</div>
	)
}

module.exports = HomeComponent;
```
- Notice how it uses the values, like `props.name`, and how it uses the function, like `onSubmit = {props.onUserSubmit}`

### Function names
- By convention, in `containers`, functions that handle user actions are prefaced with `handle`, like `handleUserSubmit(event)` in `HomeContainer.js` above.
- When these functions are passed from `containers` (in the state) to `components` (in the props), we change the name to be prefaced with `on`. Like `onUserSubmit = {this.handleUserSubmit}` in the example above.
- Then, we use this `on` prefaced function like this `onSubmit = {props.onUserSubmit}`.

### Takeaway points
- `props` are `state` that's sent to a component. It has values and functions. Components use `props` values to populate its virtual dom, and components use `props` functions to add event listeners.
- `state` is immutable! Represents data that is available to a container, and which can be passed down to components.


### P.S.
- You can change `state` with `this.setState` function. I haven't written the tutorial for this yet.