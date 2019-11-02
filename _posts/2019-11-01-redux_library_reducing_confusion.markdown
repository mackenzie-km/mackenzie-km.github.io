---
layout: post
title:      "Redux Library: Reducing Confusion"
date:       2019-11-01 20:28:08 -0400
permalink:  redux_library_reducing_confusion
---

## Mindblown
After spending all afternoon learning about the Redux Library, I decided it was time to go back to the basics.

![https://media.giphy.com/media/SJX3gbZ2dbaEhU92Pu/giphy.gif](https://media.giphy.com/media/SJX3gbZ2dbaEhU92Pu/giphy.gif)

> re·dux /rēˈdəks,ˈrēˈdəks/ (adjective) :brought back; revived.


To sort out my thoughts, here's my personal guide on how to implement redux in our React projects.


## Installing Redux
If it's not in your package.json file
```
npm install redux && npm install react-redux
```

When it's in your package.json, you can begin using React as usual 
```
npm install && npm start
```


## Setting up a store
Use redux's version of createStore()!

In your index.js file, add the following

```
import { createStore } from 'redux';
```

Now you have a method to create a store; let's create it using our application-specific reducer.

> `const store = createStore(customReducer,  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());`
> 

The last two arguments are optional if you want to use the Chrome Redux Devtools option (which, of course, you do!)

customReducer is a reducer that you created in another file. To include this reducer, make sure to import it.

```
import customReducer from './reducers/customReducer'
```



## Passing your store to components
If we had to pass in our store as a prop to every component, we would be wasting our time (and missing out on all that Redux does!)

Let's use built in methods to pass the store to all of the components.

Add this to the top of index.js

```
import { Provider } from 'react-redux’;
```

And wrap your app in this Provider 

```
2. <Provider store={store}>
3. <App />
4. </Provider>,
```

App will now receive the store you created as a prop. It can be accessed in this.props!



## Connecting components to the store 
That's great - we don't have to keep passing it in to all of the other components! But how can the components update the store?

**Wrap a component in connect()!**

> Connect() is your messenger - it connects Redux and React. You can use it to pass data and functions from Redux state to React components.

In App.js (or whatever component needs to interact with the store), put the following 

```
import { connect } from 'react-redux’;
```

This allows you to wrap a React component in Redux's connect method. 

Let's actually wrap the component now.

```
const mapStateToProps = (state) => {
return { items: state.items };
};
export default connect(mapStateToProps)(App);
```


### How it works
* export default connect(function)(Component) sends the return value of the specified function to the Component.
* Why is this useful? This is how you can tell the Component new functions or data. Namely, you can tell your component about yoru Redux state, and about how you want your dispatch function to work.
* You can send multiple functions/values in the (function) section. See below for more details.


### Connect() in a nutshell
1. Listens for a change in the Redux store. When a change happens, connect calls the function you passed in (mapStateToProps)
2. Filters out the changes relavent to the component - mapStateToProps allows you to decide which specific functions or values you want to pass on 
3. Provide the changes to the component - Redux's connect method updates your React Components with the selected data/functions

### Going even deeper: adding mapDispatchToProps

Remember how I said we can add multiple functions/values in the (function) section?

Take, for example, this code written in App.js outside of the render() call.

```
const mapStateToProps = state => {
return {
items: state.items
};
}; 

const mapDispatchToProps = dispatch => {
return {
increaseCount: () => dispatch({ type: 'INCREASE_COUNT' })
};
};

export default connect(
 mapStateToProps,
 mapDispatchToProps
 )(App);
```

Above is a cool example of mapping the state and dispatch method. 

Because you sent these values/functions to the App Component, App has access to these as props. Now you can actually use `this.props.increaseCount();` within your App component! You told the app how to dispatch it.


## Q&A of connect

This concept is a hard one, so let's go a little bit deeper.

### When is connect() fired off? 
When it notices that the store's state has changed. 

* Why is this useful? Redux knows that the store's state has changed, but React doesn't know yet. Connect acts almost as an messenger, waiting for the Redux state to change, and then passing information on to React.

### What's up with mapStateToProps?
Providing a function like this lets you tell Redux exactly what to send to React.
* For example, you could provide just First Names to React, while still maintaining email information in the Redux state in the background.

### What's up with mapStateToDispatch?
This argument is optional too, but it can be useful to specify the actions you want your dispatch function to take.
* Even if you don't pass in a second argument, you will still get access to the dispatch method with this.props.dispatch()

***

![https://media3.giphy.com/media/4EEGQFcuxRDzr4OJu7/giphy.gif](https://media3.giphy.com/media/4EEGQFcuxRDzr4OJu7/giphy.gif)

Well, that's all I got for now! Hopefully this helps clear things up - it's helping me to think clearly.


