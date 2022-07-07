# What Is JavaScript?

## Introduction
 
The name is an artifact of marketing at that time Java came out and they wanted to take advantage of the popularity of Java but his real name is Mocha.

But the official name of the language is ECMAScript is especified by TC39 and formalized by the ECMA

Each year comes out a new version. the version has been sufixed by the revision year Example: ECMAScript 2021 or otherwise abbrevieted  ES2021.

TC39 primary task is managing the oficial specification of the lengaje is regulated by a committee. The committee is regulated by people who work in companies like Microsoft, Google, Samsung, Apple. They have a meeting constantly in thats meeting the discuss about the new feactures, issues and vote.

All those proposals have 5 stages initial stage is stage 0 an the las stage is 4.
If thoose feactures reached Stage 4 that feacture is incluyed in the last version of ECMAScript. You can see thoose proposals in this [repo](https://github.com/tc39/proposals) 

Exits diferent enviroments where you can run javascrpt (Web-browser, Nade.js, robots, etc) but the most common enviroment is de Web Browser. ECMAScript have especificacions for the web browsers inside his definition. In thoose engines have the specificacions in the global scope for example alert feacture is only available in web browser engines or API's like fetech or location

JS is a multiparadigm lenguage is one of the best features Example:
* procedual: code top to down , lineal progression
* Object oriented (OO / classes) : organize the code into units called classes
* functional (FP) : function oriented 

those paradigms are good it depends how programmers approach problems and solutions

## Backwards & Forwards

It means that once something is accepted as valid JS, there will not be a future change to the language that causes that code to become invalid JS.
For TC39 is important to keep the retrocompatibility because they don't want to break the web the principal idea is: they want the developer to work with something unpredictable.
This rule have some little exception. JS has had some backwards-incompatible changes but the team studies the existing code in the web and the they decied.

JS is not Forwards compatible because it's too complicate to predice what it change in the future. HTML and CSS are Forward compatible because the only skiped the tag but in JS it's different.
JS need's todo all the thinks.
But if some feature are not able in your JS engine you can use a polyfill o you can transpile the code with babel.

If you transpile the code it convert from that newer JS syntax version to an equivalent older syntax. Example : 

Your Code
```

    if (something) { 
        let x = 3;
        console.log(x);
    }
    else {
        let x = 4;
        console.log(x);
    }


```

To

```
    var x$0, x$1; 
    if (something) {
        x$0 = 3;
        console.log(x$0);
    }
    else {
        x$1 = 4;
        console.log(x$1);
    }

```

A polyfill is code that implements a feature on web browsers that do not support the feature. Most often, it refers to a JavaScript library that implements an HTML5 or CSS web standard, either an established standard (supported by some browsers) on older browsers, or a proposed standard (not supported by any browsers) on existing browsers. Formally, "a polyfill is a shim for a browser API [Wikipedia](https://en.wikipedia.org/wiki/Polyfill_(programming)) 

Let's see how it works (Example):

In ECMAScript 2019 Have new a feautre In the Promises the ``` finally() ```

```
    // getSomeRecords() returns us a promise for some // data it will fetch
    var pr = getSomeRecords();

    // show the UI spinner while we get the data
    startSpinner();

    pr
    .then(renderRecords) // render if successful 
    .catch(showError) // show an error if not 
    .finally(hideSpinner) // always hide the spinner
```

This code don't run in JS engines that don't support ES2019 features in those case we can put a polyfill and that it works

Ex finally polyfill:

```
if (!Promise.prototype.finally) { 
    Promise.prototype.finally = function f(fn){
        return this.then( 
            function t(v){
                return Promise.resolve( fn() ) .then(function t(){
                    return v; 
                });
            },
            function c(e){
                return Promise.resolve( fn() )
                    .then(function t(){
                        throw e; 
                    });
            } 
        );
    }; 
}
```

# What’s in an Interpretation?

The people thinks javascript is a interpreted languaje but it's more complicatde than that.

Compiled lenguages usually get the proyect and produce a binary representation and executed but with JS we don't see that. This is the reason people think it is not compiled. 

In Scripting langajes o interpreted the code runs into the interpreter if the codes fails just exit and throw the error

Javascript the process change that 

    1.- parsing: don't run the program just parsing and can detect errors all the compilers do that step convert to Abstract Syntax Tree AST

    2.- Code generation:  producing an executable form. Get the AST to convert to binnary code

    3.- Compile: get the binary or intermediate representation and run the code

JS is a compiled language. The reason that matters is, since JS is compiled, we are informed of static errors (such as malformed syntax) before our code is executed.

## Web Assembly (WASM)

Preview: In 2013 engineers from Mozila Firefox runs a port of the Unreal 3 game . the can run a game with 60 fps. they adapt an optimaze the game.
Unreal engine’s code used a style of code that favored a subset of the JS language, named “ASM.js”.

Several years after ASM.js demonstrated the validity of tool- ing-created versions of programs that can be processed more efficiently by the JS engine, another group of engineers (also, initially, from Mozilla) released Web Assembly (WASM).

WASM is a representation format more akin to Assembly (hence, its name) that can be processed by a JS engine by skipping the parsing/compilation that the JS engine normally does. The parsing/compilation of a WASM-targeted program happen ahead of time (AOT); what’s distributed is a binary- packed program ready for the JS engine to execute with very minimal processing.

WASM will not replace JS. That’s a great thing, entirely orthogonal to whether some people will use it as an escape hatch from having to write JS.


## Strictly Speaking

In 2009 TC39 launch ES5 and added the strict mode is a mechanism for doing better JS code it helps to remove old bad habbits .
Strict mode is not necessarily by default.
Most JS code is worked on by teams of developers, so the strict- ness of strict mode (along with tooling like linters!) often helps collaboration on code by avoiding some of the more problematic mistakes that slip by in non-strict mode.

How we can use Example: 

```
    "use strict";
```

You can define use strict inside the scopes Example:

```
function someOperations() {
     "use strict";
    // all this code will run in strict mode
}
```

Moreover, a wide shift is happening toward more/most new JS code being written using the ES6 module format. ES6 modules assume strict mode, so all code in such files is automatically defaulted to strict mode.


