# Chapter 7: Using closures

Closures are one of the principal characteristics in javascript thanks to this caracteristic you can use more paradigms, functional programing , modules and class-oriented design.that's the reason you need to understand closures to mastering javascript.

A closure is originally a mathematical concept from labda calculus

Closure is a behavior of functions and only functions. If you aren’t dealing with a function, closure does not apply. An object cannot have closure, nor does a class have closure (though its functions/methods might). Only functions have closure.

But firt you need to know what is a scope. A scope is an area o block when you have functions or variables visibles and accessible to other code we have 2 to scopes types:
  
  * Global scope: is the global space  
  * local scope: is a restricted region o area of code 

The origin of all scopes is called lexical scope 

Example of closures:

```
  function adder(num1) {
    return function addTo(num2) {
      return num1 + num2; 
    };
  }
  var add10To = adder(10);
  var add42To = adder(42); 

  add10To(15); // 25
  add42To(9);  // 51
```

In this example we see a function calls added and you pass a number an we save the result this function retuns another function who acepts a number. if you call the variable it is a function because it return a function and you pass a value and print in the first case 25 it happens because when you execute the function it save the original value inside the scope in this case a 10 this variable still exists and still holds the original 10 value.

Even though closure is based on lexical scope, which is handled at compile time, closure is observed as a runtime characteristic of function instances.

A closure is a live link not a snapshot of the function in a spacific time you can change a value inside a function in runing time example:

```
  var studentName = "Frank";
  var greeting = function hello() {
    // we are closing over `studentName`, // not "Frank"
      console.log(
      `Hello, ${ studentName }!` );
  }
  greeting();
  // Hello, Frank!

  // later
  studentName = "Suzy";

  // later
  greeting();
  // Hello, Suzy!
```

in this Example you can see in the first time when you invoke greeting function you got 'Hello, Frank'  but if you re-asign the variable 'studentName' you get 'Hello, Suzy' it happens because in real time the compliler got the current value of 'studentName' variable thant's the reason a closure is not a snapshot is a live link of another scope.

One common case we use scope is when we use AJAX
```
function lookupStudentRecord(studentID) { 
  ajax(
    `https://some.api/student/${ studentID }`, 
    function onRecord(record) {
      console.log(`${ record.name } (${ studentID })`
    ); }
  ); 
}

lookupStudentRecord(114);
// Frank (114)
```

In this case the result of the function calls when the result of the petition is donde success this is called in one point of the future but iside the 'lookupStudentRecord' function saves the scope in real time that's the reason we can obtain diferent results.
Why then is studentID still around and accessible to the callback? Closure.

Thanks you can reference a funcion into a variable we can use closures into JS.
Whether JS functions support closure or not, this program would behave the same. Therefore, no observed closure here.
If there’s no function invocation, closure can’t be observed:

```

function greetStudent(studentName) { 
  return function greeting() {
    console.log(`Hello, ${ studentName }!`); 
  };
}
greetStudent("Kyle");
// nothing else happens because we only get the reference and we not execute
greetStudent("Sergio")()
// prints 'Hello, Sergio' because we revibe the reference and execute the reference
```

Closure is observed when a function uses vari- able(s) from outer scope(s) even while running in a scope where those variable(s) wouldn’t be accessible.

The key parts of this definition are:
  * Must be a function involved
  * Must reference at least one variable from an outer scope 
  * Must be invoked in a different branch of the scope chain from the variable(s)