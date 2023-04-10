---
title: "Closures"
datePublished: Tue Mar 28 2023 06:22:35 GMT+0000 (Coordinated Universal Time)
cuid: clfrvfex7000r09lbctawbpy2
slug: closures
tags: closure, reactjs, functional-programming, python-advanced, advanced-js

---

When we start our programming journey, the thing that really confuses us is the global identifiers. At that moment we even don't know about the global properties in a language. Then we are introduced to scopes where actually the existence of any data in memory gets explained. But why it is so important? Lets see🤓

In this blog, I am gonna start from a little description of scopes and its behaviour. Then we will move to the concept of closures and how modern react uses it behind the scenes.

## Scopes in a programming language

Scopes are the region to which a value of an identifier can be accessed. But if we go a little deep we can say it as the different blocks getting inserted into the call stack of the compiler/interpreter/jit.

In a scope, we cant declare same identifier twice. We can initialise and reinitialise it the value but we can't redeclare it. Lets see an example in java.

```java
public class ScopeExample(){
    public static void main(String[] args){
        // a scope(Global)
        int n1 = 2;
        int n1 = 1; // error as we are trying to redeclare
        n1 = 1; // not error as we are trying to reinitialise 
         for (int i=0;i<5;i++){
            //loop scope   
            // we cant create variables that are initialised in the same method
            // n1 cant be accessed outside the loop
            int n1 = 4;
        }
    }
}
```

## How scopes are resolved in a programming language?

Basically LEGB(**Local, Enclosing, Global and Built-in**) rule is followed while resolving a scope of an identifier. We get to know it while learning python but if we make a deep dive all programming languages follow the same.

**Reason?**

* Local means the identifers present in the same scope. For example, the n1 inside the for loop and it can be same as the other scopes.
    
* Enclosing scope (where actually the clousure concept comes) is the scope of outer method getting accessed by the inner methods or scope. It is explained clearly with python later on.
    
* Global scope means the region where all the scopes lie. Basically the wrapper region or outer scope.
    

Built in means the identifier that are built in. Example in java will be int , System,etc. Basically the keywords. We can override some of those properties with the help of OOPs.

## Enclosing Scope

By the name only we can get some idea that something is getting enclosed. Python will be a great language to show this

```python
def outer(outer_params):
    # enclosing scope for the inner function
    # For outer itself it is a local scope
    outer_var = 1
    def inner(inner_params):
        print(outer_var)
    return inner
```

It is looking similar to a react.js component nah 🤔?? Yes lets build the intuition.

The inner is a function can access all the variables , parameters and everything of the outer function. Literally everything can be accessed.

The magic of enclosing scope is called **closures**

> We can also say it as the global scope for the inner function.

## Closures

By looking at the example above can't we say the variables of outer are binded. Means it is seeming like we are using `self` pointer of the oops in python. Lets make it more clear

```python
def useCount():
  count = 1

  def increase_count():
    nonlocal count # to change the count val
    count+= 1

  def get_count():
    return count

  return increase_count, get_count


increase_count, get_count = useCount()

print(get_count())  # 1
increase_count()
print(get_count())  # 2
```

We can see the count value is getting updated instead of creating a new object , the same object is getting mutated. Thus we can say the count got bound to the useCount. If we go more deep , we can say the count became a state.

* **Let's go more deep**
    

```python
def useCount():
  count = 1

  def increase_count():
    nonlocal count
    count += 1

  def get_count():
    return count

  return increase_count, get_count


increase_count, get_count = useCount()

del useCount # deleting the identifier

# still we can access the state
print(get_count())  # 1
increase_count()
print(get_count())  # 2
```

Here we have deleted the useCount but still we can access the state which proves the state is bonded all the inner scopes or functions.

> **Definition of closure**  
> If you return a nested function from a function, that nested function has access to the variables defined in that function, even if that function is not active any more.  
> **Closures are created at function creation time.** Variables are organized into units of scope, such as block scope or function scope.

So we can say to some level it replaces the oop paradigm.

## How modern react uses the closure?

Modern react means functional components. Functional components are nothing but calling the calling that returns jsx or js objects.

* We can never define or use hooks outside the Component scope. This is because hooks are tend to access the component and its state and its variable. Closures remember the values of variables from previous renders - this can help prevent async bugs
    
    * useState and useEffect is a great example of it.
        
* The values defined outside the component does not get redeclared or reinitialised when the component rerenders.
    

## Conclusion

This article focused more on practicality and advanced side of a language model. I have faced alot of issues while learning closures and its application 😄😅. So I have written all things I know in summarised form.

Soon going to write another blog on decorators for which closure is tooo much important. Hope you like it.❤❤