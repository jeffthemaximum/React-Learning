### To load a react container/component on an HTML page

1. Make sure it (or something that loads it) is included in `app/assets/javascript/components.js`
2. In the `slim` file that you want the component, do something like this:

```
= react_component 'LighteningStrikesComponent'
```
