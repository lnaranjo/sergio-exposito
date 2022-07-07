# Secret Lifecycle of Variables

## When Can I Use a Variable?

Consider this code

```
    greeting();
    // Hello!

    function greeting() {
        console.log("Hello");
    }
```

This code works well but, why you can call the function greeting before his declaration?. This happened because all identifiers are registered to their respective scopes during compile time.

The term most commonly used for a variable being visible from the beginning of its enclosing scope, even though its declaration may appear further down in the scope, is called hoisting.

One key detail is that both function hoisting and var-flavored variable hoisting attach their name identifiers to the nearest enclosing function scope (or, if none, the global scope), not a block scope.

## Hoisting: Declaration vs. Expression

Consider this example with function assignments:

```
    greeting();
    // TypeError

    var greeting = function greeting() { 
        console.log("Hello!");
    };
```

This code throws a TypeError

this happening maybe at the begging of the code all variables rises and per default all variables per default his value is  undefined. or the correct answer is JS isn’t telling us that it couldn’t find greeting as an identifier in the scope. It’s telling us that greeting was found but doesn’t hold a function reference at that moment. Only functions can be invoked, so attempting to invoke some non-function value results in an error.
In both cases, the name of the identifier is hoisted. But the function reference association isn’t handled at initialization time 

## Variable Hoisting

```
    greeting = "Hello!";
    console.log(greeting);
    // Hello!
    var greeting = "Howdy!";
```

it prints "Hello!" because greeting variable hoisting and his value is undefined but when assign his value to "hello!" and print it and then after the console.log his variable is assigned to "Howdy!"

## Hoisting: Yet Another Metaphor

The hoisting (metaphor) proposes that JS pre-processes the original program and rearranges it a bit, so that all the declarations have been moved to the top of their respective scopes, before execution. 

Hoisting as a mechanism for re-ordering code may be an attractive simplification, but it’s not accurate. The JS engine doesn’t actually re-arrange the code. It can’t magically look ahead and find declarations.

I assert that hoisting should be used to refer to the compile- time operation of generating runtime instructions for the automatic registration of a variable at the beginning of its scope, each time that scope is entered.

## Re-declaration?

When a variable is re declare it keeps the same value .

See how the explicit = undefined initialization produces a different outcome than assuming it happens implicitly it assign the value to undefined.

## Constants?

The const keyword is more constrained than let. Like let, const cannot be repeated with the same identifier in the same scope. But there’s actually an overriding technical reason why that sort of “re-declaration” is disallowed, unlike let which disallows “re-declaration” mostly for stylistic reasons.

the const required a variable to be initialized.

the const variable cannot be re-assigned because it is a constant.

## Loops

JS doesn’t really want us to “re-declare” our variables within the same scope.

Consider: 

```
    var keepGoing = true; 
    while (keepGoing) {
        let value = Math.random(); 
        if (value > 0.5) {
            keepGoing = false; 
        }
    }
```

Is value being “re-declared” repeatedly in this program? Will
we get errors thrown? No.

all the rules of the scope like re-declaration are applied per scope instance . when the scope enter is a new scope instance . if you give the value variable withe var it save this variable in the global scope.

Var re-declare
let is a new assignation inside the new block

## Uninitialized Variables (aka, TDZ)

With var declarations, the variable is “hoisted” to the top of its scope. But it’s also automatically initialized to the undefined value, so that the variable can be used throughout the entire scope.

with let the variable declare and assign when the Engines arrives on the point 

if you call some thing it depends the let variable the Engine will no find the variable.

This case happens withe the const variable too .

The term coined by TC39 to refer to this period of time from the entering of a scope to where the auto-initialization of the variable occurs is: Temporal Dead Zone (TDZ).

let and const can't be hoisted because there are block variables