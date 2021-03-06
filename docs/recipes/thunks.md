# Thunks

### redux-thunk integration

In order to get the most out of writing your thunks, make sure to set up your thunk middleware using it's redux-thunk's `withExtraArgument` method like so:

```javascript
import { applyMiddleware, compose, createStore } from 'redux';
import thunk from 'redux-thunk';
import { reduxReactFirebase } from 'react-redux-firebase';
import makeRootReducer from './reducers';
import { getFirebase } from 'react-redux-firebase';

const fbConfig = {} // your firebase config

const store = createStore(
  makeRootReducer(),
  initialState,
  compose(
    applyMiddleware([
      thunk.withExtraArgument(getFirebase) // Pass getFirebase function as extra argument
    ]),
    reduxReactFirebase(fbConfig, { userProfile: 'users', enableLogging: false })
  )
);

```

## Example Thunk

After following the setup above, `getFirebase` function becomes available within your thunks as the third argument:

```javascript
const sendNotification = (payload) => {
  type: NOTIFICATION,
  payload
}
export const addTodo = (newTodo) =>
  (dispatch, getState, getFirebase) => {
    const firebase = getFirebase()
    firebase
      .helpers
      .push('todos', newTodo)
      .then(() => {
        dispatch(sendNotification('Todo Added'))
      })
  };

```
