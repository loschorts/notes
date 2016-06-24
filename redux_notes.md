0.  Supplying the Initial State
  * When you create the Redux store, its initial state is determined by the root reducer.

  * Since reducers are autonomous, each of them specifies its own initial state as the default value of the state argument.

  * Redux lets me pass the persistent state as the second argument to the create store function, and it will override the value specified by the reducers. For values not specified by the persistent state argument, the default values of the reducer will apply.

  * Whatever value we pass to create stores as a second argument is going to end up in the root reducer as the state argument instead of undefined.

  * You might get tempted to specify all the initial state tree of your app in a single place and pass it to create store, but we don't recommend this. Co-locating the initial state with the reducer definition makes it easy to test and change, so you should always strive to do that.

  * However, you can use the second argument to create store for hydrating persistent data into the store, because it was obtained from Redux itself, so it doesn't break encapsulation of reducers.
