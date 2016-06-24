0.  Supplying the Initial State
  * When you create the Redux store, its initial state is determined by the root reducer.

  * Since reducers are autonomous, each of them specifies its own initial state as the default value of the state argument.

  * Redux lets me pass the persistent state as the second argument to the create store function, and it will override the value specified by the reducers. For values not specified by the persistent state argument, the default values of the reducer will apply.

  * Whatever value we pass to create stores as a second argument is going to end up in the root reducer as the state argument instead of undefined.

  * You might get tempted to specify all the initial state tree of your app in a single place and pass it to create store, but we don't recommend this. Co-locating the initial state with the reducer definition makes it easy to test and change, so you should always strive to do that.

  * However, you can use the second argument to create store for hydrating persistent data into the store, because it was obtained from Redux itself, so it doesn't break encapsulation of reducers.

0.  Redux: Persisting the State to the Local Storage

  * I want to persist the state of the application in the localStorage using browser localStorage API. I'm going to write a function called load state and a new model that I'm going to call localStorage AS that is going to use the localStorage browser API.

  * The load state function is going to look into local storage by key, retrieve a string, and try to parse it as JSON.

  * It's important that we wrap this code into try/catch because calls to your `localStorage.getItem` can fail if the user privacy mode does not allow the use of localStorage.

  * If the serialized state is null no it means that the key does not exist so I'll return undefined to let the reducers initialize the state instead. However if the serialized state string exists I'm going to use `JSON.parse` in order to turn it into the state object. Finally in case of any errors I'm going to play it safe and return undefined to let reducers initialize the application.

  * Now that I have wrote the function to load the state, I might as well write a function that saves the state to localStorage. It's going to accept the state as an argument, and it's going to do the exact opposite thing.

    * First it serializes it to string by using JSON.stringify. This will only work if the state is serializable, but this is the general recommendation in Redux.

    * Your state should be serializable.

    * Now that both JSON.stringify and localStorage set items goal can fail so it's important that we catch any errors to prevent the app from crashing.

  * Now I am also importing the save state fiction I just wrote in the index.js. I need to save the state any time the store state changes, so I'm using the store subscribe method to add a listener that will be invoked on any state change, and I'm passing the current state of the store to my save state function.

  * To avoid problems of reducer closures being reset, NPM module called node-uuid.The function we're going to use is called V4, which is just a name of the standard. It generates a unique string ID every time, and we're going to use this ID instead of a counter.

  * We're currently call save state inside the subscribe listener so it is called every time the storage state changes. However we would like to avoid calling it too often because it uses the expensive stringify operation. To solve this I'm going to use a library called `lodash` which includes a handy utility called `throttle`. Wrapping my call back in a throttle call insures that the inner function that I pass is not going to be called more often than the number of milliseconds I specify.

  * Let's recap how we added the localStorage persistence to the tutor's app. First we created the new module with load state and save state functions. Load state looks into the localStorage, and if there is a serialized string of our state it tries to parse it as JSON.

    * If something goes wrong we return it undefined so that the app doesn't crash. We use the return value of load state as the second argument to create store so that it overrides the initial state specified by the reducers.

    * We want to be notified of the changes to the store state so we subscribe to it. We wrap our subscriber in throttle function from load dash in order to insure that it doesn't get called more often than once a second.

    * We only want to keep the todos and not the visibility filter, so we explicitly parse an object that contains just the todos from the current state. Instead save state we're going to serialize the current state to string with JSON.stringify and try to set item, but if something fails, we're just going to ignore this error so that the app doesn't crash.

Finally we change the ID generation for todos from a simple in memory counter to a function from node.uuid that generates a unique ID string every time.

0.  Perisistence Functions should be called loadState and saveState. (personal notes)

0.  Refactoring the entry point.

  * I'm extracting the logic necessary to create the store, and to subscribe to it to persist the state into a separate file. I'm calling this file configure store, and so I'm creating a function called configure store that is going to contain this logic so that the app doesn't have to know exactly how the store is created and if we have any subscribe handlers on it. It can just use the return store in the index.js file.

  * It is useful to export configure store rather than store itself, so that if you later write some tests, you can easily create as many store instances as you want. Now I can import configure store from my application's entry point, and I can use it right away without having to specify the reducer or the subscribers, because configure store takes care of that.

  *  I also want to extract the root rendered element into a separate component that I'm going to call root. It's going to accept the store as a prop, and it's going to be defined in a separate file in the components folder. I'm creating a new file called root. It needs to import React to use the JSX syntax, and I define [inaudible 1:15] functional component that just takes the store as a prop and returns an app inside a React Redux Provider. Currently, it is very simple. It just wraps the app into React Redux Provider. In the future, we can add something like a router here.

0. Adding React Router to the Project

  * To add React Router to the project, I'm running npm install --save react-router. I'm going to import the router component from it, as well as the route configuration component. Now I can replace my app with the router, and it's important that it's inside Provider so that any components rendered by the router still have access to the store.

  * Inside, I put a single route element that tells React Router that I want to render my app component at the root path. If I run the app, I can see that the router matched the path correctly and rendered the app component.

  * If you see weird symbols after a hash sign in the address bar, it means that you're using the version of React Router that doesn't yet default to the browser history, and defaults to hash history instead.

  * To fix it, you can import browser history from React Router and pass it as a history prop to Router. Unless you target very old browsers like IE9, you can always use browser history and have a clean URL in the address bar.

  * Let's recap the changes we made to add React Router to the application. I ran npm install --save react-router, and I imported the router component and the route configuration component from React Router.

  * Instead of rendering the app directly, I replaced it with a router component that has a single route at the root path that renders the app component. In order to avoid hash sign and weird symbols after it, I imported browser history, and I passed it as the history prop to the router component.

0.  Filtering Redux State with React Router Params

  * Let's recap how React Router became the source of truth for the visibility filter. In the root component, I render the router, and I only have a single route with an optional filter parameter. The filter parameter gets passed into the app component by React Router. React Router will make it available inside a special params prop

  * I am passing the filter param from the URL, or all if we are on the root path, to the visible todo list component as a filter prop. The visible todo list component now reads the filter from its props and not from the state, so it uses the filter specified by the app.

  * I change the get visible todos function to use the current filter names, all completed and active. For consistency, the footer component now also uses these names as the value of the filter prop for the filter link, and I re-implemented the filter link to use the link provided by React Router instead of our own implementation

  * Finally, I fix the dev server to correctly return index HTML no matter which path is requested, so that React Router can pick it up on the client.

  * You might think that making the router in control of the visibility filter contradicts the idea of a single-state tree. However, in practice, what matters is that there's just one single source of truth for any independent piece of data.

0.  Passing params with ReactRouter's `withRouter`

  * Passing the params from the top level of route handlers gets tedious, so I'm removing the filter prop. Instead, I'm going to find a way to read the current router params in the visible todo list itself.

  * I will add a new import code with Router from the React Router package. It's important that we use at least React Router 3.0 for it to work well with Redux. With Router, it takes a React component and returns a different React component that injects the router-related props, such as params, into your component.

  * I want the params to be available inside mapStateToProps, so I need to wrap the connect result so that the connected component gets the params as a prop.

  * Let's recap how we made the router params available inside the connected components' mapStateToProps function. We'll read the params from the ownProps argument, which corresponds to the props passed from the parent component. The params props specifically is passed by the component generated by withRouter call. This is how they end up in props of our connected component.

  * WithRouter passes any props through itself, so we're going to see both params, and any props passed from the app in the ownProps argument. Finally, I import with Router from the React Router package. It only works correctly with connect since React Router 3.0 (Now 2.4: https://github.com/reactjs/react-router/releases/tag/v2.4.0), so make sure you're on the recent version.

  * The params injected by withRouter are the same exact params that get injected by default into the route handler components. You can use both ways of getting to the params and even mix them, but with Router it's handy when you need to read the current params somewhere deep in the component tree.fs

0.  Using `mapDispatchToProps` Shorthand Notation

  * Normally, mapDispatchToProps accepts the dispatch function as an argument so that you can return the props through inject into the React component that each can dispatch certain actions using the dispatch function.

  * However, it is very common that the arguments passed through the callback props are passed through to the action creators in the same order. In this case, rather than write mapDispatchToProps function yourself, it can pass a configuration object that maps the names of the callback props to the corresponding action creator functions.

0.  
