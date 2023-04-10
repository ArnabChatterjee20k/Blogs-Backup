---
title: "Making data fetching more verbose in JS"
seoTitle: "Correct way to fetch data using fetch api and js"
seoDescription: "Proper design pattern for fetching data using js and fetch api. Make your data fetching process more interactive."
datePublished: Sat Jan 28 2023 14:13:17 GMT+0000 (Coordinated Universal Time)
cuid: cldg19hbv000009l6f23a4uvu
slug: making-data-fetching-more-verbose-in-js
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1674911422571/3edfc466-21bc-4e05-9489-23d46ceaecea.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1674915560441/1587c505-0814-427b-ac76-56d1bd18ade8.png
tags: design-patterns, javascript, axios, fetch-api, wemakedev

---

## Why Is Fetching Data Correctly Important?

What do we all expect from a modern application? We only expect that the application is at least interactive and user-friendly. For maintaining interactive correctly fetching data plays the most important role as the maximum of the time our data is coming from an api or web service.

The whole blog is done using fetch api but the method is 100% applicable for `axios` as well. Just replace `fetch api` with `axios.`

## Commonly Used Techinique

Lets try to fetch todos for a todo application.

1. Call the api with fetch function which then returns a promise
    
2. do promise chaining using then to view the data
    
3. use catch to view the error
    

And that's all🤓

```javascript
const req = fetch('https://jsonplaceholder.typicode.com/todos')
req
    .then(data => data.json())
    .then(jsonData => console.log(jsonData))
    .catch(err => console.log(err.message))
```

### What is wrong with this approach?

We all viewed the documentation of JS but we all missed one thing in the note section the catch function does not catch errors for status codes. And that's the biggest bug here in this code.

Generally, we all consider status codes except 200 as error status codes. But that is not the case of `fetch api.`

## Correct Approach

We can correct this approach without any external package. Below is the implementation.

```javascript
const req = fetch('https://jsonplaceholder.typicode.com/todos')
req
   .then(data => {
      if (!data.statusText === "ok") {
        throw Error(data.status)
      }
      return data.json()
    })
    .then(jsonData => console.log(jsonData))
    .catch(error => {
      const errCode = parseInt(error.message)
      console.log(errorFactory(errCode))
    })

function errorFactory(errCode) {
  switch (errCode) {
    case 403:
      return "No access to todo"
    case 404:
      return "Todo not found"
    case 500:
      return "Please Try again later"
    default:
      return "Some error occured"
  }
}
```

### What is actually good in this approach?

> `.statusText` returns ok if the response status code is 200.

* Here we are throwing an error for all status codes except 200. So one problem is solved.
    
* Now When we are throwing the error we are not throwing any random error we throwing the error code as the message of the error.
    
* We also built an `errorFactory` function which return messages based on some common error codes.
    
* Now when showing the error just use this function and use the errocode as the argument and show them to the user.
    

## Using Async Await

Suppose we are making a feature where by just clicking a todo item a particular todo we get the todo in a full screen

```javascript
const getTodo = async (id) => {
  const url = `https://jsonplaceholder.typicode.com/todos/${id}`
  const res = await fetch(url)
  if (!res.ok) {
    throw new Error(res.status);
  }
  const data = await res.json();
  return data;
};

const todoClickHandler = async => (id){
    try{
        const todo = await getTodo(id)
        console.log(todo)
    }
    catch(error){
      const errCode = parseInt(error.message)
      console.log(errorFactory(errCode))
    }
}
```

### Making the approach more generic

We can make a function handler that will take the function as an argument and other arguments of a function as another argument then call them in a try-catch block.

By doing this we don't have to write try-catch block each time

```javascript
async function wrapper(fnc,...args){
    try{
        const data = await fnc(...args)
        return [data,null] // sending the data
    }
    catch(error){
      const errCode = parseInt(error.message)
      console.log(errorFactory(errCode))
      return [null,error] // sending the error
    }
}
```

Now we can use this runner function to run any function without any headache of repeated try-catch. A single wrapper to wrap all your async function. No need to use try catch block every time.

> " ... " is the spread and rest operator. Yes, two names.

## Conclusion

We can follow these design patterns in our codebases to cut off the issues of repetitions and follow a strict DRY principle and keep our codebase clean and top-notch.