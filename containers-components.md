# Containers and Components

### Overview

- **Containers** are JavaScript objects that hold the `state` and `logic` of that piece of the virtual DOM.
- **Components** are logicless and stateless. Their only goal is to present an element to the page.

### Example

- Jingo breaks these out into seperate files. He's got this kind of file structure:

```
app/
	components/
		HomeComponents.js
	containers/
		HomeContainer.js
```
Then, in `HomeContainer.js` he has:
```
import React from 'react';
import HomeComponent from '../components/HomeComponent';

const HomeContainer = React.createClass({
	render(){
		return(
			<HomeComponent/>
		)
	}
})

export default HomeContainer;
```

And in `HomeComponents.js` he has:
```
import React from 'react';

const styles = {
	centering: {
		textAlign: 'center'
	}
}

function HomeComponent(props){
	return(
		<div className="container" style={styles.centering}>
			<h1>This Is Your Movie App!</h1>
			<div className="row">
		    <form className="col s10 offset-s2 m4 offset-m4"
		    	onSubmit = {props.onSubmitMovie}>
		    	<input 
		    		placeholder="Enter Movie or TV Show Title" 
		    		type="text"
		    		className="validate"
		    		onChange = {props.onUpdateMovie}/>
		    	<input 
		    		type="submit"
		    		hidden/>
		    </form>
		  </div>
		</div>
	)
}

module.exports = HomeComponent;
```
- **Take away points**: 
	- The components and containers are seperated in their own files.
	- Components are presentational.
	- Containers have logic and state.

##### QUESTION!
- Why are Jingo's files `.js` and not `.jsx`?

##### QUESTION!
- Jingo has these broken into seperate folders and files, but shakacode does not. Why?
- For example, shakacode has this: https://github.com/shakacode/react-webpack-rails-tutorial/blob/master/client/app/bundles/comments/components/SimpleCommentScreen/SimpleCommentScreen.jsx
- Notice in shakacode's that he has a presentational component in the same file has container logic.