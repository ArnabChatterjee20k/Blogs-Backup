---
title: "Implementing getters and setters in real life applications - Using OOPS"
datePublished: Tue Feb 28 2023 15:57:40 GMT+0000 (Coordinated Universal Time)
cuid: cleofn56m000109la3b7l8ssq
slug: implementing-getters-and-setters-in-real-life-applications-using-oops
tags: design-patterns, oops, wemakedevs, getter-and-setter, wemakedev

---

## Introduction

Getters and Setters are one of the most underrated concepts of OOPs. We can use it in our applications to make the logic as well as code readability more clear and also we can make the workflow of the design patterns more verbose. **I used this property for building a production grade backend. So you can use this as a reference. Moreover if python is your language then it will be definitely a turning point of your codebase.**

But before moving to the concept of getters and setters we need to understand what is contructor of a class. The codes are written in python but the logic can be applied to any programming language.

Prerequisite - Basic knowledge of OOPs like what is a class , an object and how the initialization occurs, encapsulation and abstraction. That's it . This much easy the topic is going to be.

### Constructor

* A constructor is a function rather a special method that is called while we initialize an object of that particular class.
    
* Here all the properties get binded to the object. In python, we access them using the `self` keyword and in JavaScript using `this` keyword.
    
* So, it basically constructs the object by adding properties to it.
    

In python `__init__` , a dunder method is the constructor function.

```python
 class Human:
    # constructor 
    def __init__(self,legs,hands):
        self.legs = legs
        self.hands = hands

normal_human = Human(4,2)
print(normal_human.legs,normal_human.hands)
```

So we can see how it's all done. Now suppose we want to assign a property which is dependent on another property.

```python
 class Human:
    # constructor 
    def __init__(self,legs):
        self.legs = legs
        self.hands = self.legs//2; # making the hands dependent on legs

normal_human = Human(4)
print(normal_human.legs,normal_human.hands) # 4,2 

# legs changed but hands not changed 👀
normal_human.legs = 5
print(normal_human.legs,normal_human.hands) # 5,2 
```

So now we can see the legs and hands are dependent but making changes in the legs not changing the hands. Here all the chaos occurs.

## Possible Solution using methods

What can be a possible solution?? Well we can build a method which will calculate the value then return the modified value.

```python
 class Human:
    # constructor 
    def __init__(self,legs):
        self.legs = legs

    def hands(self):
        return self.legs//2;

normal_human = Human(4)
print(normal_human.legs,normal_human.hands()) # 4,2 

normal_human.legs = 6
print(normal_human.legs,normal_human.hands()) # 6,3
```

It is running fine. But what if we want to make this simpler? Calling methods is not an ideal case every time.

Well here only the concept of the getters and setters. Using methods as attributes or variable or properties.

## Getters

Methods for accessing an attribute and getting used as an attribute is called **getter**. So instead of making a public attribute , we will make a private attribute first so that it is not accessible directly even by the instances or objects.

Then we will use property decorator to make it a getter.

```python
class Human:
    # constructor 
    def __init__(self,legs):
        self.legs = legs
        self.__hands = 0

    @property
    def hands(self):
        return self.legs//2;
        
# simpler code ✌
normal_human = Human(4)
print(normal_human.legs,normal_human.hands) # 4,2 

normal_human.legs = 6
print(normal_human.legs,normal_human.hands) # 6,3
```

## Setters

Now what if we want to set the hand explicitly. Methods for setting or updating an attribute and getting used as an attribute is called **setter**.

```python
class Human:
    # constructor 
    def __init__(self,legs):
        self.legs = legs
        self.__hands = 0

    @property
    def hands(self):
        return self.legs//2;

    @hands.setter
     def hands(self,value):
        self.___hands = value
# simpler code ✌
normal_human = Human(4)
print(normal_human.legs,normal_human.hands) # 4,2 

normal_human.hands = 5
print(normal_human.legs,normal_human.hands) # 6,5
```

## Using getters and setters with sqlalchemy ORM to store hashed password easily

> What if instead of legs and hands we have a **password** property which should be actually a hashed value of the provided password?
> 
> Normally We do this using two properties which is unnecessary. Lets simplify it.

```python
class Doctor(db.Model):
    id = db.Column(db.Integer,primary_key = True)
    name = db.Column(db.String(30),nullable=False)
    __password = db.Column(db.String)
    
     @property
    def password(self):
        return self.__password
    
    @password.setter
    def password(self,value):
        self.__password = generate_password_hash(value)
```

Now technically you built a class which accepts password and only shows you the hashed password. Even changing password will lead to storing of hashed password.

## Conclusion

Sometimes without using these concepts it is hard to visualise what the codebase actually wants to do. Yes I know functional oriented programming is a go and generally used by popular tech stacks but if you want to structure you code and secure it then OOP is the only key.