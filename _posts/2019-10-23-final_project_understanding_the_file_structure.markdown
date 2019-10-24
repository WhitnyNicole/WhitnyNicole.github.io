---
layout: post
title:      "Final Project: Understanding the File Structure"
date:       2019-10-24 00:22:12 +0000
permalink:  final_project_understanding_the_file_structure
---


My final project had a frontend utilizing React and Redux. Here is a simple breakdown of the file structure in my project with some notes about the purpose of each:

App.js:
* contains my componentDidMount lifecycle method which gets all my interviews and reviews
* contains all my routes with an exact path and a component to render for each

Actions Directory: 
* includes two actions: interviews and reviews
* actions are just functions
* returns a function that takes dispatch as an argument. With dispatch, you make an API call/fetch request. I made a fetch request to my Rails API
* once you receive the data, you dispatch to your reducers by returning a type and payload
* fetch returns a promise, which is resolved using `.then`. The first thing that is returned is the response object which we call `,json` on to parse out the JSON data
* after the fetch request, you dispatch to the reducer

Reducers Directory:
* includes an Interview Reducer and a Review Reducer 
* each reducer includes a state and an action
* the state is normally defaulted to some value, which is the initial state of the app
* includes a switch case statement which switches on the action.type which comes from the action

Container Components:
* these components are concerned with how things work
* provide the data and behavior to presentational and/or other container components
* they often contain state and serve as data resources
* usually generated using higher order components such as `connect()`
* connect is used to connect this component to the Redux store

Presentational Components:
* concerned with how things look
* don't specify how data is loaded or mutated
* receive data from props
* rarely contain state
* usually written as functional components unless they need state
* usually contain some DOM markup and styling 

