# Reward amount from modal to pledge form

- in `app/assets/javascripts/components/rewards/RewardModalComponent.js.jsx` there is the form for the reward modal.
- User clicks a reward
- `handleRewardClick(reward)` updates the `amount` in this component's state.
- User clicks the `submit button` in the form in this component
- this fires `this.handleSubmit`, because of the `onSubmit` event handler on the form.
- `handleSubmit` calls `this.props.setRewardPledge(this.state.amount)` 
- `this.props.setRewardPledge` comes from `app/assets/javascripts/containers/pledge-container.js.jsx`
- **QUESTION** - how does `app/assets/javascripts/containers/pledge-container.js.jsx` pass `setRewardPledge` into `app/assets/javascripts/components/rewards/RewardModalComponent.js.jsx` as a prop?
- **ANSWER** - in `app/assets/javascripts/containers/pledge-container.js.jsx`, `setRewardPledge` is inside `const mapDispatchToProps = (dispatch, ownProps) => {` ... then, `mapDispatchToProps` gets exported at the bottom of that file with:
```
module.exports = {
  mapStateToProps: mapStateToProps,
  mapDispatchToProps: mapDispatchToProps
}
```
- **ANSWER CONTINUED** - that's how the `mapDispatchToProps` gets exported from `app/assets/javascripts/containers/pledge-container.js.jsx` ... it gets into `app/assets/javascripts/components/rewards/RewardModalComponent.js.jsx` because at the bottom of that file, we have:

```
module.exports = connect(
  PledgeContainer.mapStateToProps,
  PledgeContainer.mapDispatchToProps
)(RewardModalComponent);
```
- **ANSWER CONTINUED** - so that `connect` situation is enough to (add?) (replace?) the props in `app/assets/javascripts/components/rewards/RewardModalComponent.js.jsx` with the props sent in from `app/assets/javascripts/containers/pledge-container.js.jsx`

- so `this.props.setRewardPledge(this.state.amount)` gets called inside the `handleSubmit` function in `app/assets/javascripts/components/rewards/RewardModalComponent.js.jsx`
- `setRewardPledge` calls `dispatch(actions.setRewardPledge(rewardPledgeAmount))`
- And also because we have:
```
module.exports = {
  setRewardPledge: setRewardPledge,
  ...
```
- At the bottom of `app/assets/javascripts/actions.js.jsx`
- this calls `setRewardPledge` in `app/assets/javascripts/actions.js.jsx`
- If you're not logged in, `setRewardPledge` calls `dispatch(showModal({name: 'login', type: 'signup'}))`
- If you are logged in, `setRewardPledge` calls `dispatch(setRewardPledgeAmount(rewardPledgeAmount));`
- this calls `setRewardPledgeAmount` also in `app/assets/javascripts/actions.js.jsx`
- `setRewardPledgeAmount` does:
```
return {type: SET_REWARD_PLEDGE_AMOUNT, rewardPledgeAmount}
```
- This calls a reducer!
- The reducer is in `app/assets/javascripts/reducers/rewards-data.js.jsx` and looks like this:
```
function rewardsData(state = {}, action = {}) {
  switch (action.type) {
    case 'SET_REWARD_PLEDGE_AMOUNT':
      return Object.assign({}, state, {
        rewardPledgeAmount: action.rewardPledgeAmount
      })
    default:
      return state
  }
}
```
- **QUESTION** - how does doing `return {type: SET_REWARD_PLEDGE_AMOUNT, rewardPledgeAmount}` call that reducer?
- **ANSWER** - At the bottom of `app/assets/javascripts/reducers/rewards-data.js.jsx` we have:
```
export default rewardsData;
```
- **ANSWER CONTINUED** - then, in `app/assets/javascripts/reducers.js.jsx` we have `var rewardsData = require("reducers/rewards-data");` at the top, and at the bottom we have:
```
export default combineReducers({
  ...
  rewardsData
})
```
- **ANSWER CONTINUED** - Then, in `app/assets/javascripts/app/react-loader.js`, we have `const experiment = require('../reducers');` at the top.
- This is where we create the store, doing: 
```
let store = createStore(
  experiment,
  initialState,
  applyMiddleware(
    thunkMiddleware
  )
);
```
- **ANSWER CONTINUED** - we then register this store on the page in `app/assets/javascripts/app/react-loader.js` with:
```
ReactDOM.render(
  <Provider store={store}>
    <HeaderContainer />
  </Provider>,
  elem
);
```
- That's how the `setRewardPledgeAmount` in `app/assets/javascripts/actions.js.jsx` is able to call the reducer in `app/assets/javascripts/reducers/rewards-data.js.jsx`
- Once that reducer gets called, it adds the rewardPledgeAmount to the store state.
- Now that it's in the store state, we do this in `app/assets/javascripts/containers/pledge-container.js.jsx`
```
const mapStateToProps = (state, ownProps) => {
  ...
  return {
      ...
      rewardsData: state.rewardsData.rewardPledgeAmount
  }
}
```
- `mapStateToProps` automatically gets passed the store state, which contains `rewardsData`
- This gets `rewardsData` into the initial state for `PledgeContainer`
- `app/assets/javascripts/components/pledge-creator/pledge-creator.js.jsx` has access to the state in `app/assets/javascripts/containers/pledge-container.js.jsx` as props, because of `mapStateToProps`
- In `app/assets/javascripts/components/pledge-creator/pledge-creator.js.jsx` we pass rewardsData to `PledgeForm` with
```
<PledgeForm 
  ...
  rewardsAmount={this.props.rewardsData}
/>
```
- `PledgeForm` then passes `rewardsAmount` to `PledgeAmountInput` with this:
```
<PledgeAmountInput
  ...
  rewardsAmount={this.props.rewardsAmount}
/>
```
- `PledgeAmountInput` is in `app/assets/javascripts/components/pledge-creator/pledge-amount-input.js.jsx`
- It sets the value for the pledge input field with:
```
<input
  ... 
  value={this.initialPledgeAmount()} 
/>
```
- `initialPledgeAmount` is a function that does this:
```
initialPledgeAmount(){
  if (this.props.rewardsAmount) {
    return this.props.rewardsAmount;
  } else {
    return this.props.data.amount;
  }
},
```