# Let and Const

### const

 - ES6 syntax for declaring a variable, like `var`. `const` tells us that the variable is immutable.

### let
- many nuances, documented well here http://stackoverflow.com/a/11444416/4906871
- Main difference is that `var` is scoped to the nearest function block, and `let` is scoped to the nearest enclosing block.
- Example:
```
function ingWithinEstablishedParameters() {
    let terOfRecommendation = 'awesome worker!'; //function block scoped
    var sityCheerleading = 'go!'; //function block scoped
}
```
- In that use case, their identical.
- Example 2:
```
function allyIlliterate() {
    //tuce is *not* visible out here

    for( let tuce = 0; tuce < 5; tuce++ ) {
        //tuce is only visible in here (and in the for() parentheses)
        //and there is a separate tuce variable for each iteration of the loop
    }

    //tuce is *not* visible out here
}

function byE40() {
    //nish *is* visible out here

    for( var nish = 0; nish < 5; nish++ ) {
        //nish is visible to the whole function
    }

    //nish *is* visible out here
}
```
- In that example, you can see how `let` gets scoped to the `for loop`, but `var` is scoped to the whole function.

### Difference between `let` and `const`
- This guy takes the difference real seriously: https://mathiasbynens.be/notes/es6-const
- `const` assigns a value to a variable name, then ensures that no `rebinding` will happen. As far as I can tell, `rebinding` means reassignment.
- This seems to be the only difference between `let` and `const`.
