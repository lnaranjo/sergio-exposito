# Digging to the Roots of JS

## Iteration

The iterator pattern defines a data structure called an “iter- ator” that has a reference to an underlying data source (like the query result rows), which exposes a method like next(). Calling next() returns the next piece of data (i.e., a “record” or “row” from a database query).

The importance of the iterator pattern is in adhering to a standard way of processing data iteratively, which creates
cleaner and easier to understand code, as opposed to having every data structure/source define its own custom way of handling its data.

ES6 standardized a specific protocol for the iterator pattern directly in the language by defining a next() method whose return an object called an iterator result; the object has value and doneproperties, where done is a boolean that is false until the iteration over the underlying data source is complete.

## Consuming Iterators

Since ES6 have more mechanism to cosume data, one of thoose mechanism is the for..of loop Example:

```
    // given an iterator of some data source:
    var it = /* .. */;
    // loop over its results one at a time
    for (let val of it) {
        console.log(`Iterator value: ${ val }`);
    }
    // Iterator value: ..
    // Iterator value: ..
    // ..
```

Another mechanism to consume iterators is the ... operator this actually has two symmetrical forms
    * spread
    * rest

### spread indicator 

    To spread an iterator, you have to have something to spread it into. There are two possibilities in JS: an array or an argument list for a function call.

    ```
    // spread an iterator into an array, 
    // with each iterated value occupying 
    // an array element position.
    var vals = [ ...it ]; 

    // or function spread 
    dosomthing(...it);

    ```
    boath cases follow the iterator-consumption protocol  to retrive all avaliable values

## Iterables

Iterable is a value that can be iterated over. The protocol automatically creates an iterator instance to its completion 

We can find the iterables in some data structure/collection types in JS 

    * strings 
    * arrays 
    * maps 
    * sets

Example of array iterator 

```
    // an array is an iterable
    var arr = [ 10, 20, 30 ];
    for (let val of arr) {
        console.log(`Array value: ${ val }`);
    }
    // Array value: 10
    // Array value: 20
    // Array value: 30

    // an array using iterator consumption via the ... spread operator

    var arrCopy = [ ...arr ];

    // you can iterate strings because is an iterator ex

    var greeting = "Hello world!"; 
    var chars = [ ...greeting ];
    chars;
    // [ "H", "e", "l", "l", "o", " ",
    //   "w", "o", "r", "l", "d", "!" ]
```

A Map data structure uses objects as keys, associating a value (of any type) with that object. Maps have a different default iteration than seen here, in that the iteration is not just over the map’s values but instead its entries. An entry is a tuple (2-element array) including both a key and a value.

```
    var buttonNames = new Map(); 
    buttonNames.set(btn1,"Button 1"); 
    buttonNames.set(btn2,"Button 2");
    for (let [btn,btnName] of buttonNames) { 
        btn.addEventListener("click",function onClick(){
            console.log(`Clicked ${ btnName }`); 
        });
    }
```

In this example have some Iterators on of thoose is the for...of and [btn,btnName] is calling array destrucuring to breack down each consumed tupleint the respective key 

## Closure

All JS developers needs to meet how the closures works because the mayority of the javascript code works with closures.
A Closure is  when a function remembers and continues to access variables from outside its scope, even when the function is executed in a different scope.

    * closure is part of the nature of a function.
    * to observe a closure, you must execute a function in a different scope than where that function was originally defined.

```
    function greeting(msg) { 
        return function who(name) {
            console.log(`${ msg }, ${ name }!`); 
        };
    }
    var hello = greeting("Hello");
    var howdy = greeting("Howdy"); 
    hello("Kyle");
    // Hello, Kyle!

```

When yo execute greeting function that function returns who function if you execute that function you can obtain the console log with the name and who.

With the closures you can keep values inside the other scope and you can do some actions Example:
```
    function counter(step = 1) { 
        var count = 0;
        return function increaseCount(){ 
            count = count + step; 
            return count;
        }; 
    }
    var incBy1 = counter(1); 
    var incBy3 = counter(3);

    incBy1(); //1 
    incBy1(); //2
    incBy3(); //3 
    incBy3(); //6 
    incBy3(); //9
```

you can set a conter with diferents increese value and the keep that value.

Closures is using when you work with async code Example:

```
    function getSomeData(url) { 
        ajax(url,function onResponse(resp){
            console.log(`Response (from ${ url }): ${ resp }`); 
        });
    }
```

## this Keyword

One of JS’s most powerful mechanisms is also one of its most misunderstood: the this keyword. One common mis- conception is that a function’s this refers to the function itself. 

When a function is defined, it is attached to its enclosing scope via closure. Scope is the set of rules that controls how references to variables are resolved.

Scope is static and contains a fixed set of variables available at the moment and location you define a function, but a function’s execution context is dynamic. Example:
```
    function classroom(teacher) {
        return function study() {
            console.log(`${teacher} says to study ${this.topic}`);
        };
    }

    var assignment = classroom("Kyle"); // this undefined

    var homework = {
        topic: "JS",
        assignment: assignment,
    };

    homework.assignment();
    // Kyle says to study JS

    var otherHomework = { topic: "Math" };
    assignment.call(otherHomework);
    // Kyle says to study Math

```

## Prototypes

Prototype as a linkage between two objects; the linkage is hidden behind the scenes, though there are ways to expose and observe it. This prototype linkage occurs when an object is created; it’s linked to another object that already exists.

The purpose of this prototype linkage (i.e., from an object B to another object A) is so that accesses against B for properties/methods that B does not have, are delegated to A to handle. Delegation of property/method access allows two (or more!) objects to cooperate with each other to perform a task.

