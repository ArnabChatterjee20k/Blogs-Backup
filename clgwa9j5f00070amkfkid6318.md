---
title: "Server Session Based Authentication over JWT authentication?? 🤔🤔"
seoTitle: "Server-Side Sessions vs. JWT: Which is Better for Your Application?"
seoDescription: "Get a comprehensive comparison of server-side sessions vs. JWT authentication methods to make an informed decision for your application. Read now."
datePublished: Tue Apr 25 2023 13:08:41 GMT+0000 (Coordinated Universal Time)
cuid: clgwa9j5f00070amkfkid6318
slug: server-session-based-authentication-over-jwt-authentication
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1682428065624/7b4df188-c59d-437a-ba19-e034eb6a0910.png
tags: security, backend, databases, wemakedevs, json-web-tokens-jwt

---

Backend security is one of the crucial topics in backend development but most of us neglect it not because we want to but lack of resources. If we analyze the backend resources available to us the correct direction for a good backend is really very less as compared to the frontend.

But how do I know this concept of security? Mostly because of my current position as a backend intern in one of the reputed startups of India and also due to some open source repos and also some good articles. I was going through the codebase found this and evaluated my concepts practically by the senior engineers. So you can believe on me and this blog. Also I was exploring firebase and from their also I explored the same thing.

# Why do we need sessions and jwts?

Mostly the backend and frontend communication works through `http` protocol which by itself is a stateless protocol. What is stateless? Stateless means for communication no data needed to be stored of frontend and backend on any side.

But sometimes for a better user experience, we want the data to get stored to cut out the repetitions. A very common example will be user authentication where we don't want our user to relog or register again and again to access the services. So what do we do? We generate some symbol or some data and pass it to frontend in terms of cookies or tokens which can be later used by the backend whether the user is verified or not.

The token can be expirable, can be stored as cookies, can be stored in servers as session data, and can be stored in the data storage of the client.

# JWT - the best and worst

Let us kick the complex scary technical definition of jwt and understand in a simple way . JWT or JSON Web Token is basically an encrypted string containing 3 parts -

* Header -&gt; **Containing token type and algorithm**
    
* Payload -&gt; **The data in form of js objects or any hashable type data**
    
* Signature -&gt; **The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way**.
    

> eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV\_adQssw5c
> 
> We can see the dots. The 3 parts are divided by the dots. The middle part is the data.

But where are we sending this jwt? Obviously to frontend. And frontend is where we can see everything. You are thinking oh no one can see the data present in the jwt. And with that confidence, you put password and other private information in the jwt and stored them in localstorage. Now what??

Let's decode the string with javascript and hack your backend😎😎

```javascript
// data or payload is present in the middle portion
atob("eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ")
```

Tada your backend is hacked 🎉.

### What to put in payload jwt?

Never put passwords and sensible information in jwt. You can put emails , ids or uuids or other non sensitive data.

### Still, is JWT safe and 100% worthy?

No still is not safe. You are giving email and uuid from the database. Basically giving one of the entities of your application. Hit and trial method using that email and can be hacked using rainbow tables.

Another thing that can happen is that JWT is not cross-domain. This means suppose you have used dark light theme or user choice in jwt. If the device changed or browser cache or memory is cleared user needs to set it up again.

# Intuition of the Solution

I told you all the advantages and disadvantages of the jwt. Now lets try to build a better solution.

* Everything in the frontend is visible. To solve this we can build some proxy or some indirect link.
    
* We can decode jwts easily. What if we provide an abstract layer to the payload. What if the information in the jwt is an indirect link and the data present their is not an part our application.
    
* Providing a cross device storage which is feasible , expirable and can be easily bootedt up and also we dont have to query database as queries are costly in production.
    
* Server side is visible. So we can rely upon it.
    

All of these are provided by server side sessions 👀👀

# Server Side Sessions

Here the concept is that you maintain a cache in the primary memory so that searching can be fast. Lets understand it by building our workflow of a registration endpoint.

### Registration Endpoint example

We can use any database depending on the package we are using(redis , memecache,mongodb , etc). But redis will be a ideal choice as all of the package supports redis for caching.

* Store the user in the database
    
* If using a sql database flush the user to get the id. If nosql then you can directly get it.
    
* Generate a jwt token or a simple payload using the id and email.
    
* Now we are having an extra layer of session in server side. Store the generated token in the server side. Now you send that id to the frontend.
    

### Login Endpoint Logic

* Parse the token from the post request data and search for it in the session.
    
* If found give the id , authorisation rights else send bad authorisation error.
    

### Advantages

* Here since we are using a proxy id or entity so the end user dont know what the id maps to the session
    
* It is cross-domain.
    
    * Till the user in the session the data will be valid.
        
    * We can access from any domain
        
* We can store some internal or external tokens for that user dynamically and securely
    
* If you are building microservices then all the services can access the session cache and will be on same page.
    

# Practical Examples other than Auth

Suppose you are using an external api which is generating some frequently changing tokens like refresh tokens then server side sessions plays a great role. Giving them access to the frontend is a great risk.

* Example - google apis
    

# Conclusion

I know jwts are simple and a go-to solution and server-side sessions are complex and we need to manage it but when it comes to security no mercy should be made.

Also, it depends on what kind of application we are building.

I hope you liked my blog and got some value🎉. Keep learning and keep coding ❤️❤️