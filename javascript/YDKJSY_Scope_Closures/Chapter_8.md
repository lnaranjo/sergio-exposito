# Chapter 8: The Module Pattern

## Encapsulation and Least Exposure (POLE)

The goal of encapsulation is the bundling or colocation of information (data) and behavior (functions) that together serve a common purpose.
The spirit of encapsulation can be realized in something as simple as using separate files to hold bits of the overall program with common purpose. 
Another key goal is the control of visibility of certain aspects of the encapsulated data and functionality. Recall from the least privilege principle (POLE), which seeks to defensively guard against various dangers of scope overexposure; these affect both variables and functions. In JS, we most often implement visibility control through the mechanics of lexical scope.

## What Is a Module?

A module is a collection of related data and functions. It maintains some information over time, along with functionality to access and update that information.

## Namespaces (Stateless Grouping)

A namespace is a group of functions without data, you don’t really have the expected encapsulation a module implies. The better term for this grouping of stateless functions is a namespace:

```
// namespace, not module
var Utils = { 
  cancelEvt(evt) {
    evt.preventDefault();
    evt.stopPropagation();
    evt.stopImmediatePropagation();
  }, 
  wait(ms) {
    return new Promise(function c(res){ 
      setTimeout(res,ms);
    }); 
  },
  isValidEmail(email) {
    return /[^@]+@[^@.]+\.[^@.]+/.test(email);
  } 
};
```

In this case ecach function are state-independient

## Data Structures (Stateful Grouping)

Even if you bundle data and stateful functions together, if you’re not limiting the visibility of any of it, then you’re stopping short of the POLE aspect of encapsulation:

```
// data structure, not module
var Student = { 
  records: [
    { id: 14, name: "Kyle", grade: 86 },
    { id: 73, name: "Suzy", grade: 87 },
    { id: 112, name: "Frank", grade: 75 },
    { id: 6, name: "Sarah", grade: 91 }
  ],
  getName(studentID) {
    var student = this.records.find( student => student.id == studentID);
    return student.name; 
  }
};
```