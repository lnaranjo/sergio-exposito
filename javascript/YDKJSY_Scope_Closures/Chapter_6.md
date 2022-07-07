# Chapter 6: Limiting Scope Exposure

In this chapter we will see how we should be using diferents levels of scope to organize program variables. this helps you to reduce scope over-expouse

## Least Exposure

Software engineering articulates a fundamental discipline, typically applied to software security, called “The Principle of Least Privilege” (POLP).

And a variation of this principle that applies to our current discussion is typically labeled as “Least Exposure” (POLE).

POLP expresses a defensive posture to software architecture: components of the system should be designed to function with least privilege, least access, least exposure.

Principaly we need to try to make our functions with the minimal required code or call to other functions. we try to do the function with lower level function this helps us to modify our functions with more freedom and have more freedom to write our functions.

When variables used by one part of the program are exposed to another part of the program, via scope, 
  * Naming Collisions: a common error is if you try to usea a variable it existe in our scope and you don't kmow where can you affect another functionality 
  * Unexpected Behavior: if you expose variables/func- tions whose usage is otherwise private to a piece of the program, it allows other developers to use them in ways you didn’t intend, which can violate expected behavior and cause bugs.
  * Unintended Dependency: try to no expose variables/functions it helps to protect ypur funcitons imagine if some other dev take your variable o function and modifies. This maybe can brake your code

## Hiding in Plain (Function) Scope

We’ve already seen the let and const keywords, which are block scoped declarators; we’ll come back to them in more detail shortly. this helps you a lot because this variables are only create into the scope you declare. In this case a 'var' dosn't help you because your var's goes to the global scope thanks to the hoisting 

## Invoking Function Expressions Immediately

Invoked Function Ex- pression (IIFE) helps you when we want to create a scope to hide variables/functions.

```
  // outer scope
    (function(){
      // inner hidden scope
    })();
  // more outer scope

```

this helps you to isolate your code into a specific scope

## Scoping with Blocks

The reason to use let or const in your features it helps you to isolate variables inside in an block/function and don't allow to share variables into another scope this helps you to follow The Principle of Least Privilege patternt this helps you to prevent bugs and help into your code organization 

## var and let

var attaches to the nearest enclosing function scope, no matter where it appears. Even though var is inside a block, its declaration is function-scopped . var is better if you want to communicate functions scoped . let both communicates (and achieves!) block-scoping where var is insufficient. As long as your programs are going to need both function-scoped and block-scoped variables, the most sensible and readable. approach is to use both var and let together, each for their own best purpose.

## Where To let?

use var only into inside block and var into top level scope. POLE pattern helps you to select if you use var or let 

## What’s the Catch?

The catch clause has used an additional (little-known) block- scoping declaration capability:

```
  try { 
    doesntExist();
  }
  catch (err) {
    console.log(err);
    // ReferenceError: 'doesntExist' is not defined
    // ^^^^ message printed from the caught exception
    let onlyHere = true;
    var outerVariable = true; 
  }
  console.log(outerVariable);     // true
  console.log(err);
  // ReferenceError: 'err' is not defined
  // ^^^^ this is another thrown (uncaught) exception
```

The err variable declared by the catch clause is block-scoped to that block. This catch clause block can hold other block- scoped declarations via let. But a var declaration inside this block still attaches to the outer function/global scope.

## Function Declarations in Blocks (FiB)

We’ve seen now that declarations using let or const are block-scoped, and var declarations are function-scoped. So what about function declarations that appear directly inside blocks? As a feature, this is called “FiB.”