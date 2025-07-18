# MyLearnings
A curated log of my daily tech learnings, new concepts, theories, and noteworthy industry updates — across software development, system design, ML, and more.

##### What is a Middleware in a web application?
It is a layer that sits between 2 parts of your system

In Backend: Between the **request** & the **response**

In Frontend: Between the **dispatched actions** and the **store** in Redux

Why do we need it?

Backend: To process things before hitting the final endpoint

Frontend: To handle async operations/API calls, to dispatch other actions based on the side effects

Middleware is used mainly to handle the side effects

Common Redux middlewares are:

- redux-thunk (simple async logic with functions)
  
- redux-saga (complex async flows using generators)

- redux-logger (logs every action & state change)

###### What is createSagaMiddleware()?

It's a function from the redux-saga library

It creates a middleware that sits between redux **actions** & the **reducer** and runs your generator functions (**sagas**) when specific actions are dispatched.

###### Note: Redux reducers must be pure - they can't do things like API calls, waits, background retries or debounced updates, etc

Thus, we use **redux-saga** to handle all this logic **outside the reducer**, while still keeping things **tied to Redux**

createSagaMiddleware() is needed so that Redux knows **how to use sagas**.

1) Create the saga middleware:

const sagaMiddleware = createSagaMiddleware();

2) Add it to Redux

const store = createStore(
  rootReducer,
  applyMiddleware(sagaMiddleware)
);

3) Start the sagas

sagaMiddleware.run(rootSaga);

The above code will watch for specific redux actions and when they occur, it triggers the respective generator function that you define to handle them (e.g., making API calls)

###### What is put in sagas?

It is like a dispatch in Redux. It **sends an action** to the **redux store**.

To send success, failure, or progress updates **back to Redux from your saga**.

###### What is select in sagas?

select lets you **read data** from the **Redux store** inside your saga.

UseCases: Check if user is logged in, Get current filter/page settings, Avoid calling APIs if data is already present

###### What is takeLatest in sagas?

takeLatest watches for an action and runs the saga, but:

If another action of the **same type** comes in before the first one finishes, it cancels the first one and **runs only the latest**.

UseCases: Search bar, login, fetch data on button click — if the user clicks multiple times, run only the last one.

###### What is fork in sagas?

It's a **non blocking** call to start a **new saga task** in the background

“Start this task now, but don’t wait for it to finish — keep going with the current one.”

Eg:

function* rootSaga() {
  yield fork(accountSaga);   // runs in background
  yield fork(depositSaga);  // runs in background
}

accountSaga and depositSaga both run concurrently and do not block each other.

Fork is used to run in **parallel and independently**

