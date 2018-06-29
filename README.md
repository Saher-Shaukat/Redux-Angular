# Redux-Angular
Implemented Redux in Angular

Redux is a predictable state container for JavaScript apps which makes it possible to use a centralized state management in your application. A centralized state is just data you’re using by more than one component (application level state).

To initiate a new Angular 4 project we can use Angular CLI:

$ ng new angularedux-todo

## Install Redux for Angular

$ npm install redux @angular-redux/store --save

## Implementing Store, Actions and Reducer

### **Store:**

Create the new file src/app/todo.ts and insert the following implementation:
```
export interface ITodo {
    id: number;
    description: string;
    responsible: string;
    priority: string;
    isCompleted: boolean;
}
```

Add the following import statement to store.ts:

*import { ITodo } from './todo';*

In store.ts in the project folder src/app and insert the following piece of code.

```
export interface IAppState {
    todos: ITodo[];
    lastUpdate: Date;
}
export const INITIAL_STATE: IAppState = {
    todos: [],
    lastUpdate: null
}
```

Here you can see that two properties are defined:

1. todos: as an array of type ITodo to contain all of our todo items
2. lastUpdate: as Date type to contain the information when the todos array has been updated

At the same time we’re defining the INITIAL_STATE object of type IAppState. INITIAL_STATE is implementing the interface IAppState and initializing the properties todos with an empty array and lastUpdate with null. 

### **Action**
Create a new file src/app/actions.ts and define the following four action types:

```
export const ADD_TODO = 'ADD_TODO';
export const TOGGLE_TODO = 'TOGGLE_TODO';
export const REMOVE_TODO = 'REMOVE_TODO';
export const REMOVE_ALL_TODOS = 'REMOVE_ALL_TODOS';
```
 
 ### **Reducer**
Finish the implementation of the the reducer function in store.ts and make use of the action types as you can see in the following:

```
export function rootReducer(state: IAppState, action): IAppState {
    switch (action.type) {
        case ADD_TODO:
            action.todo.id = state.todos.length + 1;    
            return Object.assign({}, state, {
                todos: state.todos.concat(Object.assign({}, action.todo)),
                lastUpdate: new Date()
            })
        
        case TOGGLE_TODO:
            var todo = state.todos.find(t => t.id === action.id);
            var index = state.todos.indexOf(todo);
            return Object.assign({}, state, {
                todos: [
                    ...state.todos.slice(0, index),
                    Object.assign({}, todo, {isCompleted: !todo.isCompleted}),
                    ...state.todos.slice(index+1)
                ],
                lastUpdate: new Date()
            })
        case REMOVE_TODO:
            return Object.assign({}, state, {
                todos: state.todos.filter(t => t.id !== action.id),
                lastUpdate: new Date()
            })
        case REMOVE_ALL_TODOS:
            return Object.assign({}, state, {
                todos: [],
                lastUpdate: new Date()
            })
    }
    return state;
}
```

## Activating The Application Store

Now let’s activate the store for our application. First add the following import statement to the top of app.module.ts:

*import { NgRedux, NgReduxModule } from '@angular-redux/store';*

Next, add NgReduxModule to the imports array of @NgModule as well.

We need to add one further import statement to import IAppState, rootReducer and INITIAL_STATE from store.ts:

*import { IAppState, rootReducer, INITIAL_STATE } from './store';*

The activation of the store is done by adding a constructor to the AppModule class, injecting NgRedux into that constructor and then calling the configureStore method of the NgRedux service:

```
export class AppModule {
    constructor (ngRedux: NgRedux<IAppState>) {
        ngRedux.configureStore(rootReducer, INITIAL_STATE);
    }
}
```

The configureStore method takes two parameter. As the first parameter we’re passing in our reducer function rootReducer. The second parameter is an object containing the initial state of the store. In our case we’ve defined INITIAL_STATE already so that we can pass in that object here.

