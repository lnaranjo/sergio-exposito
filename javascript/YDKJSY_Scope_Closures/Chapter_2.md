# Illustrating Lexical Scope

This chapter will illustrate scope with several metaphors. The goal here is to think about how your program is handled by the JS engine in ways that more closely align with how the JS engine actually works.

## Marbles, and Buckets, and Bubbles... Oh My!

In JS code each block is considered a independent scope

the first scope is named 'globl scope' 
```
    // outer/global scope: RED
    var students = [
        { id: 14, name: "Kyle" }, 
        { id: 73, name: "Suzy" }, 
        { id: 112, name: "Frank" }, 
        { id: 6, name: "Sarah" }
    ];
    function getStudentName(studentID) {
        // function scope: BLUE
        for (let student of students) { 
            // loop scope: GREEN
            if (student.id == studentID) { 
                return student.name;
            } 
        }
    }

```

this code have 3 sopes RED, BLUE and GREEN thoose scope are indepent

## A Conversation Among Friends

Another useful metaphor for the process f analyzing variables and the scopes.

JS engine have those parts:

    * Engine: responsible for start-to-finish compilation and execution of our javascript program.
    * compailer: one of Engine's friends; handles all the dirty work of parsing and code-generation
    * scope manager: collects and maintains a lookup list of all the declared variables/identifiers and enforces a set of rules as to how these are accessible to currently excuting code.

The first thing Compiler will do with this program is perform lexing to break it down into tokens, which it will then parse into a tree (AST).

Once Compiler get to code generation, there's more detail to consider than may be obvious. A reasonable assumption would be that compiler will produce code for the first statement.

## Nested Scope

When it comes time to execute the getStudentName() function, the Engines asks for a scope manager instance for that function's scope, and it will then proceed to look up parameter to assign the 73 argument value to, and so on.

Each scope gets its own Scope Manager instance each time that scope is executed (one or more times). Each scope auto- matically has all its identifiers registered at the start of the scope being executed this is called variable hosting.

At the beginning of scope if any identifier came from a function declaration. that variable is automatically initialized. Any identifier came for var, let and const.
variable automatically init to underfined in case of var but with let/const declare when parse the variable 


## Lookup Failures

When Engine exhausts all lexically available scopes (moving outward) and still cannot resolve the lookup of an identifier, an error condition then exists.

## Undefined Mess

If the variable is a source, an unresolved identifier lookup is considered an undeclared variable, which always results in a ReferenceError being thrown.
The error message for an undeclared variable condition in most JS enviroments, will look like.
Reference Error: XYZ is not defined.” The phrase “not defined” seems almost identical to the word “undefined,” as far as the English language goes.

To perpetuate the confusion even further, JS’s typeof opera- tor returns the string "undefined" for variable references in either state:

```
    var studentName;
    typeof studentName; // "undefined"
    typeof doesntExist; // "undefined"
```
## Global... What!?

If the variable is a target and strict-mode is not in effect, a confusing and surprising legacy behavior kicks in. The troublesome outcome is that the global scope’s Scope Manager will just create an accidental global variable to fulfill that target assignment!

```
    function getStudentName() {
        // assignment to an undeclared variable :( nextStudent="Suzy";
    }
    getStudentName();
    console.log(nextStudent);
    // "Suzy" -- oops, an accidental-global variable!
```

Assigning to a never-declared variable is an error, so it’s right that we would receive a ReferenceError here.

