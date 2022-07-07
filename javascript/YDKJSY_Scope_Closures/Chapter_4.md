# Around the Global Scope

This chapter is about global scope and we see deeply how it works

## Why global Scope ?

the most js apps are composed of multiple individual JS files. so how it run those files together in a single context or runtime?

If your are using ES modules these files are loaded individually by the JS environment. Each module then imports references to whichever other modules it need to access. the files cooperate through these shared imports.

Second, if you're using a bundler in your progress like webpack o parser all the files are concatenated together before delivery to the browse.
with all the pieces of the application co-located in a single file, some mechanism is necessary for each piece to register a name to be referred to by other pieces, as well as some facility for that access to occur.

In the bundle bundler each file is enclose in a wrapper function like this example :

```
    (function wrappingOuterScope(){
        var moduleOne = (function one(){
        // ..
        })();
        var moduleTwo = (function two(){
        // ..
        function callModuleOne() { 
            moduleOne.someMethod();
        }
        // ..
        })(); 

        function callModuleOne() { 
            moduleOne.someMethod();
        }
    })();
```
in this file you have inside the wrappingOuterScope function it is an "application-wide scope" in his top lave we have the global scope

inside this function you have the modules stored inside a variables .

if you want to call a module you have a function and this function call de module stored inside the variable.


there are another way is calling in different scripts

if you have 2 files and those files is contains the module inside a IIFE like that Example:

```
    var moduleName = (function two(){ // ..
        function callModuleOne() { 
            moduleOne.someMethod();
        }
    // ..
    })();
```
 A browser environment, each top-level variable declaration will end up as a global variable, since the global scope is the only shared resource between these two separate files—they’re independent programs, from the perspective of the JS engine.

 ## Where Exactly is this Global Scope?

The global scope is located in the outermost portion of a file; that is, not inside any function or other block. But it’s not quite as simple as that.Different JS environments handle the scopes of your pro- grams, especially the global scope, differently. 

## Browser “Window”

It is the most pure JS Env can be run in is as a standalone .js file loaded in a web page environment in a browser. I don’t mean “pure” as in nothing automatically added—lots may be added!— but rather in terms of minimal intrusion on the code or interference with its expected global scope behavior. Each js file is loaded into a ```<script> ``` tag in the DOM element. when the script is loading it will be pass to de global scope of the app.

## Globals Shadowing Globals

Where one variable declaration can override and prevent access to a declaration of the same name from an outer scope.

he difference between a global variable and a global property of the same name is that, within just the global scope itself, a global object property can be shadowed by a global variable.

## DOM Globals

I asserted that a browser-hosted JS environment has the most pure global scope behavior we’ll see. However, it’s not entirely pure.

the first step is locate the id in the DOM and include in the global scope in the browser. this occurs only in the browser

## What’s in a (Window) Name?

Another global scope oddity in browser-based JS:

```
    var name = 42; 
    console.log(name, typeof name);
    // "42" string
```

window.name is define into window.name (global) but in the console log if you identify it prints another type of variable it happens because inside the console log the name variable convert to string because console log find number it convert to string it separate the variable into another scope.

## Web Workers

Web Workers are a web platform extension on top of browser- JS behavior. it permits run code in a separate thread.

Web Worker programs run on a separate thread, they’re restricted in their communications with the main application thread, to avoid/limit race conditions and other complications. Web Worker code does not have access to the DOM.

Web workers can't access to the DOM or some API in the browser like navigator.It don't share the global scope with the main JS program.

In a web worker, the global object reference ir typically name mas using self

```
    var studentName = "Kyle"; 
    let studentID = 42;
    function hello() {
        console.log(`Hello, ${ self.studentName }!`);
    }
    self.hello();
    // Hello, Kyle!
    self.studentID;
    // undefined
```

## ES modules (ESM)

One of the most obvious impacts of using ESM is how it changes the behavior of the observably top-level scope in a file. Example:

```
    var studentName = "Kyle";
    function hello() {
        console.log(`Hello, ${ studentName }!`);
    } 
    hello();
    // Hello, Kyle!
    export hello;
```

If that code is in a file that’s loaded as an ES module, it will still run exactly the same. However, the observable effects, from the overall application perspective, will be different.

If you import the code like a module it only includes the export variable "hello" the other variables are inside only includes inside of his context

## Node

Your Node programs is never actually the global scope, the way it is when loading a non-module file in the browser.

Node has from its beginning supported a module format referred to as “CommonJS”, which looks like this:

```
    var studentName = "Kyle";
    function hello() {
        console.log(`Hello, ${ studentName }!`);
    } 
    hello();
    // Hello, Kyle!
    module.exports.hello = hello;

```
Before processing, Node effectively wraps such code in a function, so that the var and function declarations are contained in that wrapping function’s scope, not treated as global variables.

As noted earlier, Node defines a number of “globals” like require(), but they’re not actually identifiers in the global scope (nor properties of the global object). They’re injected in the scope of every module, essentially a bit like the parameters listed in the Module(..) function declaration.

So how do you define actual global variables in Node? The only way to do so is to add properties to another of Node’s automatically provided “globals,” which is ironically called global. global is a reference to the real global scope object, somewhat like using window in a browser JS environment.

```
    global.studentName = "Kyle";
    function hello() {
        console.log(`Hello, ${ studentName }!`);
    } 
    hello();
    // Hello, Kyle!
    module.exports.hello = hello;
```

Here we add studentName as a property on the global object, and then in the console.log(..) statement we’re able to access studentName as a normal global variable.

## Global This