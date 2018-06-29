# Redux-Angular
Implemented Redux in Angular

Redux is a predictable state container for JavaScript apps which makes it possible to use a centralized state management in your application. A centralized state is just data you’re using by more than one component (application level state).

To initiate a new Angular 4 project we can use Angular CLI:

$ ng new angularedux-todo

# Installation Redux for Angular

$ npm install redux @angular-redux/store --save

# Implementing Store, Actions and Reducer

Store:

Here you can see that two properties are defined:

1) todos: as an array of type ITodo to contain all of our todo items
2) lastUpdate: as Date type to contain the information when the todos array has been updated