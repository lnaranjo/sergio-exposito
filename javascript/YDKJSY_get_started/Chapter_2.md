# Chapter 2: Surveying JS

## Each File is a Program
Each .js file in your proyect or website is a program, Javascript Engine parse and compile each file into your proyect and run it individualy thats the reason if one file fails during the compilation. your proyect will run partialy.

If your using a module bundle it join all the files into one single file When this happends, JS treats this single file combined file as the entire program.

Since ES6, JS has also supported a module format in addition to the typical standalone JS program format. Modules are also file-based. If a file is loaded via module-loading mechanism such as an import statement or a ```<script type=module>``` tag, all its code is treated as a single module.

## Values

Values come in two forms in JS: primitive and object.

* Primitive: strings, booleans, numbers, symbols, null and undefined.
* Object: Arrays and Objects.

Values are embedded in programs using literals:

```
greeting("My name is Kyle.");

```

In this program, the value "My name is Kyle." is a primi- tive string literal; strings are ordered collections of characters, usually used to represent words and sentences.

## Value Type Determination

You can use typeoff for distinguishing values 

```
    typeof 42; // "number"
    typeof "abc"; // "string"
    typeof true; // "boolean"
    typeof undefined; // "undefined"
    typeof null; // "object JS Language bug"
    typeof { a: 1 }; // "object"
    typeof [1, 2, 3]; // "object"
    typeof function hello() {}; // "function"

```

## Declaring and Using Variables

There are 3 differents type of variables 

    * var: Is called block scoping all vars goes to global scope 
    * let: Is a block variable is similar to var but only exist in his scope
    * const : Is like a let  but has an additional limitation that it must be given a value at the moment it’s declared, and cannot be re-assigned a different value later.

## Functions

We should consider “function” to take the broader meaning of another related term: “procedure.” A procedure is a collection of statements that can be invoked one or more times, may be provided some inputs, and may give back one or more outputs.

In Javascript we have 2 kinds to declare a functions:

    * declaration: all declaration functions when you compile thats function goes to globl scope
    * expression:  you can assign a function like a variable

## Equal...ish

there are two ways to compere values in JS in of thoose is a normal comparison with '==' and strict comparison with '==='

Normal comparison compares values but it dosn't care the type of value Ex: 

``` 42 == 42 ```

Strict comparison compares the value and the type of value.
 
 Examples: 

```
    3 === 3.0 //true       // true
    "yes" === "yes"        // true
    null === null;         // true
    false === false;       // true
    42 === "42";           // false
    "hello" === "Hello";   // false
    true === 1;            // false
    0 === null;            // false
    "" === null;           // false
    null === undefined;    // false

```

there are 2 special cases 
    * NaN === NaN; // false 
    * 0 === -0;    // false

In the case of NaN, the === operator lies and says that an occurrence of NaN is not equal to another NaN. In the case of -0 (yes, this is a real, distinct value you can use intentionally in your programs!), the === operator lies and says it’s equal to the regular 0 value.

## JS Organization

Two major patterns for organizing code (data and behavior) are: Classes and Modules.

Both patterns can be use at the same time,

## Object Oriented

A class in a program is a definition of a “type” of custom data structure that includes both data and behaviors that operate on that data. 

```
    class Page { constructor(text) {
            this.text = text; }
            print() { console.log(this.text);
        } 
    }
    class Notebook { constructor() {
        this.pages = []; }
        addPage(text) {
            var page = new Page(text); this.pages.push(page);
        }
        print() {
            for (let page of this.pages) {
                page.print();
            }
        } 
    }
    var mathNotes = new Notebook(); 
    mathNotes.addPage("Arithmetic: + - * / ..."); 
    mathNotes.addPage("Trigonometry: sin cos tan ...");
    mathNotes.print();

```
## Class Inheritance

that is de capacity to heredate variables and methods of another class. Yo need to user ``` super(); ``` in the constructor if you want to call the constructor of the other class

```
    class Book extends Publication { 
        constructor(bookDetails) {
            super( bookDetails.title, bookDetails.author, bookDetails.publishedOn);
            this.publisher = bookDetails.publisher; this.ISBN = bookDetails.ISBN;
        }
        print() {
            super.print();
            console.log(`Publisher: ${ this.publisher } ISBN: ${ this.ISBN } `);
        } 
    }

    class BlogPost extends Publication { 
        constructor(title,author,pubDate,URL) {
            super(title,author,pubDate);
            this.URL = URL; }
        print() { 
            super.print();
            console.log(this.URL); 
        }
    }
```

thoose class use the Both Book and BlogPost use the extends clause to extend the general definition of Publication to include additional behavior. The super(..) call in each constructor delegates to the parent Publication class’s constructor for its initial- ization work, and then they do more specific things according to their respective publication type (aka, “sub-class” or “child class”).

## Modules
    The module pattern has essentially the same goal as the class pattern, which is to group data and behavior together into logical units. Also like classes, modules can “include” or “access” the data and behaviors of other modules, for cooperation sake.

## Classic Modules

ES6 added a module syntax form to native JS syntax, which we’ll look at in a moment. But from the early days of JS, modules was an important and common pattern that was leveraged in countless JS programs, even without a dedicated syntax.

```
    function Publication(title, author, pubDate) {
        var publicAPI = {
            print() {
            console.log(`Title: ${title} By: ${author} ${pubDate}`);
            },
        };

        return publicAPI;
    }

    function Book(bookDetails) {
        var pub = Publication(
            bookDetails.title,
            bookDetails.author,
            bookDetails.publishedOn
        );
        var publicAPI = {
            print() {
            pub.print();
            console.log(
                `Publisher: ${bookDetails.publisher} ISBN: ${bookDetails.ISBN}`
            );
            },
        };

        return publicAPI;
    }

    var YDKJS = Book({
        title: "You Don't Know JS",
        author: "Kyle Simpson",
        publishedOn: "June 2014",
        publisher: "O'Reilly",
        ISBN: "123456-789",
    });

    YDKJS.print();
    // Title: You Don't Know JS
    // By: Kyle Simpson
    // June 2014
    // Publisher: O'Reilly
    // ISBN: 123456-789
```

## ES Modules
    ES modules (ESM), introduced to the JS language in ES6, are meant to serve much the same spirit and purpose as the existing classic modules just described, especially taking into account important variations and use cases from AMD, UMD, and CommonJS.

    you don't need to instance the ES module you can import with "import"

    if you want to export a variable o function you need to add the keyboard export

    Ex:
```
    function printDetails(title, author, pubDate) {
        console.log(`Title: ${title} By: ${author} ${pubDate}`);
    }

    export function create(title, author, pubDate) {
        var publicAPI = {
            print() {
                printDetails(title, author, pubDate);
            },
        };
        return publicAPI;
    }

```
    import { create as createPub } from "publication.js";

    function printDetails(pub, URL) {
        pub.print();
        console.log(URL);
    }

    export function create(title, author, pubDate, URL) {
    var pub = createPub(title, author, pubDate);

    var publicAPI = {
        print() {
        printDetails(pub, URL);
        },
    };

    return publicAPI;
    }
```

```
    import { create as newBlogPost } from "blogpost.js";

    var forAgainstLet = newBlogPost(
        "For and against let",
        "Kyle Simpson",
        "October 27, 2014",
        "https://davidwalsh.name/for-and-against-let"
    );

    forAgainstLet.print();
    // Title: For and against let
    // By: Kyle Simpson
    // October 27, 2014
    // https://davidwalsh.name/for-and-against-let
```