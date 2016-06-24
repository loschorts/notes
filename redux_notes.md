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

  * 
