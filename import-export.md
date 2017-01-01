# Exporting and importing

- React best practice is to export one component per file. 
- Example: 
```
export default class SimpleCommentScreen
```
- This exports a component called `SimpleCommentScreen`. 

- To import it, you'd do:
```
Import SimpleCommentScreen from './components/Forms`
```
- Assuming your container was in a file called `Forms.jsx`

##### QUESTION!!!

- Jingo does:
```
export default HomeContainer;
```
- At the bottom of a file. But `https://github.com/shakacode/react-webpack-rails-tutorial` does:

```
export default class SimpleCommentScreen extends BaseComponent {
  constructor(props, context) {
    super(props, context);
    this.state = {
      $$comments: Immutable.fromJS([]),
      isSaving: false,
      fetchCommentsError: null,
      submitCommentError: null,
    };

    _.bindAll(this, 'fetchComments', 'handleCommentSubmit');
  }
```

- Why does shakacode do `class` but Jingo doesn't?
- Why does shakacode do their export at the top of a file but Jingo does it at the bottom? Is there any functional difference?

##### Other news about importing and exporting

- The above example assumes **one export per file**. That's what `default` does. 
- You can have multiple named exports in a single file. 
- Example:
```
export class Template {}
export class AnotherTemplate {}
```
- These would be in the same file.
- To use these components in another file, you'd do:
```
import {Template, AnotherTemplate} from './components/templates'
```
- Notice the `{}` syntax here. You don't need this with the `default` export.

- Jingo uses a different method for exporting multiple functions per file. **NOTE** he does this for functions, haven't tested with containers or components.
- In part 4 of his tutorial, he exports two functions with:
```
module.exports = {
    singleSearch : singleSearch,
    multiSearch : multiSearch
}
```
- When I tried to do this like this:
```
export class singleSearch {}
export class multiSearch {}
```
- ^ That threw an error, but the `module.exports` works.

**QUESTION!!**
- Why is that?
