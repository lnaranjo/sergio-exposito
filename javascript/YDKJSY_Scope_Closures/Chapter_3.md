# The Scope Chain


## “Lookup” Is (Mostly) Conceptual

If some variables don't find in the current scope it asks to the another scope if the variable or function exists. The scope manager controls those thinks. during the runtime.

If no declaration is found, that’s not necessarily an error. Another file (program) in the runtime may indeed declare that variable in the shared global scope.

## Shadowing

So if you need to maintain two or more variables of the same name, you must use separate (often nested) scopes. And in that case, it’s very relevant how the different scope buckets are laid out.

```
    var studentName = "Suzy";
        
    function printStudent(studentName) { 
        studentName = studentName.toUpperCase(); 
        console.log(studentName);
    }
    printStudent("Frank");
    // FRANK
    printStudent(studentName);
    // SUZY
    console.log(studentName);
    // Suzy  
```

In this example we see the re-declaration lives only inside de function scope thats the reason reason when you call the function printStudent again you obtain the student name with uppercase

that reassign only occurs inside the printStudent scope

## Global Unshadowing Trick

if yo want to coll the original value declare in the global scope in case if you are running JS in the navigator. Ex

```
    var studentName = "Suzy";
    function printStudent(studentName) { 
        console.log(studentName); 
        console.log(window.studentName);
    }
    printStudent("Frank");
    // "Frank"
    // "Suzy"

```

In this code you have two studentName declaration in the global-scope in this case is window and another inside the printStudent function and yo can call de global variable window and print that variable in the global scope

Other forms of global scope declarations do not create mirrored global object properties:

```
    var one = 1;
    let notOne = 2; 
    const notTwo = 3; 
    class notThree {}

    console.log(window.one); // 1
    console.log(window.notOne); // undefined
    console.log(window.notTwo); // undefined
    console.log(window.notThree);  // undefined
```

Variables (no matter how they’re declared!) that exist in any other scope than the global scope are completely inaccessible.

## Copying Is Not Accessing

Consider:

```
    var special = 42;
    function lookingFor(special) { 
        var another = {
            special: special
        };
        function keepLooking() {
            var special = 3.141592;
            console.log(special); 
            console.log(another.special); // Ooo, tricky! 
            console.log(window.special);
        }
        keepLooking();
    }
    lookingFor(112358132134);
    // 3.141592
    // 112358132134
    // 42

```
The function print 3 results 3.141592, 112358132134, 42. The first result occurs because when un calling keepLooking that declare special in this scope and obtain the result.Next result prints because dont fin variable another and this scope ask to the next scope if it have this variable and get the reference and the last one got the variable from the global scope.

## Illegal Shadowing

Not all combinations of declaration are allow. let can shadow var but var cannot shadow let:

```
    function something() {
        var special = "JavaScript";
        {
            let special = 42; // totally fine shadowing
            // ..
        } 
    }

    function another () {
        {
            let special = 'JavaScript';
            {
                var special = "JavaScript";
            }
        }
    }

```

The syntax error description in this case indicates that special has already been defined, but that error message is a little misleading—again, no such error happens in some- thing(), as shadowing is generally allowed just fine.

The real reason it’s raised as a SyntaxError is because the var is basically trying to “cross the boundary” of (or hop over) the let declaration of the same name, which is not allowed.

let (in an inner scope) can always shadow an outer scope’s var. var (in an inner scope) can only shadow an outer scope’s let if there is a function boundary in between.

## Function Name Scope

```
    var askQuestion = function(){ 
        // ..
    };

```

askQuestion being created. But since it's a function expression.
Function expression is a function definition used as value instead of a standalone declaration.

the difference between function declarations and function expressions is what happens to the name identifier of the function. Consider a named function expression:

```
    var askquestion = function ofTheTeacher() {

    }
```

For formal function declarations, the name identifier ends up in the outer/en- closing scope, so it may be reasonable to assume that’s true here. But ofTheTeacher is declared as an identifier inside

```
    var askQuestion = function ofTheTeacher() { 
        console.log (ofTheTeacher);
    };
    askQuestion();
    // function ofTheTeacher()...
    console.log(ofTheTeacher);
    // ReferenceError: ofTheTeacher is not defined
    
```

If you invoke the function askQuestion it return a ofTheTeacher function. this function don't exist in the scope that's the reason you can't access to the variable directly.

## Arrow Functions

ES6 added an additional function expression form to the language, called “arrow functions”:

```
var askQuestion = () => {

};
```

The function body is optional in some cases. And when the { .. } are omitted, a return value is sent out without using a return keyword.

That function are lexically anonymous , meaning they have no directly related identifier that references the function.

arrow functions are not the same to the standard functions.
arrow functions have the same lexical scope rules as function functions do. An arrow function, with or without { .. } around its body, still creates a separate, inner nested bucket of scope.
