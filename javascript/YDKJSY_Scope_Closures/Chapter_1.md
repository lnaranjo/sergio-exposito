# What’s the Scope?

JS function are calling first-class values. because they can be assigned and aassed around like numbers and strings but these function can hold and access variales.they mantain the original scope no matter where the function is called the maintain the original scope .This is called "closure"

Modules have a code pattern characterized by public methods that have privileged access (closures)

# Compiled vs. Interpreted

Code compilation set of steps that process the text of your code and turn it into a list of instructions the computer can uderstand.

Interpretation performs a similar task to compilation in that transforms your program int machine-understandable instructions.
Compiled programs compiled all the program but interpreted code get the souce code and transform line by line.

# Compiling Code

We focus on how the compilation process works because Scope is primarily determined during compilation. this is the key in mastering scope.

a program is processed by a compiler in three basic stages.

1. Tokenizing/Lexing: breaking up a string of characters into meaningful chunks, called tokens.

```

var a = 2;

//this broken into the following tokens

var,a,=,;
```

The difference between tokenizing and lexing is subtle and academic, but it centers on whether or not these tokens are identified in a stateless or stateful way.

2. Parsing: taking tokens and turning into de Abstract Syntac Tree(AST). Example

var a = 2; might start with a top-level node called VariableDeclaration, with a child node called Identifier (whose value is a), and another child called AssignmentExpression which it- self has a child called NumericLiteral (whose value is 2).

3. Code generation: Take the AST and turning it into executable code.
The JS engine takes the just described AST for var a = 2; and turns it into a set of machine instructions to actually create a variable called a (including reserving memory, etc.), and then store a value into a.


JS don't have abundance of time to perform

JS compilation doesn’t happen in a build step ahead of time, as with other languages. It usually must happen in mere microseconds (or less!) right before the code is executed. To ensure the fastest performance under these constraints, JS engines use all kinds of tricks (like JITs, which lazy compile and even hot re- compile); these are well beyond the “scope” of our discussion here.

# Required: Two Phases

The separation of a parsing/compilation phase from the sub- sequent execution phase is observable fact, not theory or opin- ion. While the JS specification does not require “compilation” explicitly, it requires behavior that is essentially only practical with a compile-then-execute approach.

consider this program 

```

var greeting = "Hello"; 
console.log(greeting);
greeting = ."Hi";
// SyntaxError: unexpected token .

```

if you run this code prints de syntax error and dont print eht console.log it happens because JS previewsly parse the code.

Early Errors

```

    console.log("Howdy");
    saySomething("Hello","Hi");
    // Uncaught SyntaxError: Duplicate parameter name not
    // allowed in this context
    function saySomething(greeting,greeting) { "use strict";
        console.log(greeting);
    }

```

the initial log not working because when the code is parsing it detects a syntax error with the dplicate msg it detects therror because the strict mode is able inside the function.

Hoisting

Consider this code

```
function saySomething() {
    var greeting = "Hello"; {
    greeting = "Howdy"; // error comes from here let greeting = "Hi";
    console.log(greeting);
}}
    saySomething();
    // ReferenceError: Cannot access 'greeting' before
    // initialization
```

this error comes because you declare greeting variable afte it is asigne and not take the another greeting variable because in this block you have a block variable this error has named Temopal Dead Zone (TDZ).

That's the reason JS converting the code into most efficient binnary if this JS is interpreted the complation time will working slow.

# Compiler Speak

Let's see how Javascript engine identificates variables and determines the scopes

```
    var students = [
    { 
        { id: 14, name: "Kyle" }, 
        { id: 73, name: "Suzy" }, 
        { id: 112, name: "Frank" }, 
        { id: 6, name: "Sarah" }
    ];
    function getStudentName(studentID) { 
        for (let student of students) {
            if (student.id == studentID) {
                return student.name;
            }
        }
    }

    var nextStudent = getStudentName(73); 
    console.log(nextStudent);
```
Other than declarations, all occurrences of variables/identi- fiers in a program serve in one of two “roles”: either they’re the target of an assignment or they’re the source of a value.


How do you know if a variable is a target? Check if there is a value that is being assigned to it; if so, it’s a target. If not, then the variable is a source.

For the JS engine to properly handle a program’s variables, it must first label each occurrence of a variable as target or source. We’ll dig in now to how each role is determined.

# Targets

What makes a variable a target? Consider: 

    students = [ // ..

This statement is clearly an assignment operation; remember, the var students part is handled entirely as a declaration at compile time, and is thus irrelevant during execution; we left it out for clarity and focus. Same with the nextStudent = getStudentName(73) statement.
