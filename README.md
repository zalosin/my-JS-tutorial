# Introduction to JavaScript
## Intro
JavaScript is high-level, often just-in-time compiled, and multi-paradigm programming language. [Wikipedia]
--
## Variables
We declare them using `let` or `const` ( `var` is old and should not be used) can hold pretty much anything ranging from values, objects, functions etc.

- `let`
    - default way of declaring a variable
    - can be reassigned
    - can be uninitialized
    - lifecycle only in the block it is defined in
- `const`
    - declares a constant
    - MUST be initialized with a value when declared
    - can't be changed
    - lifecycle only in the block it is defined in
- `var` please don't use this, legacy, deprecated etc.
    - here just for educational purposes
    - does not have block scope its either global or function scope, see below

### Naming conventions
- JS uses `camelCase` for variables, functions, objects etc., `MACRO_CASE` for constants where we store values to be used throughout our code and `PascalCase` for classes and interfaces
for example:
```JS
const MAIN_COLOR = "#FF7F00";
let mainRect = document.getElementById('rect');
mainRect.style.fill = MAIN_COLOR;
class SleepData{};
```
### Const > let
Always use `const` unless you need to change the value of what is stored in the variable
### Code blocks

- brackets create a code block
    ```JS
    {
        const a = 3;
        console.log(a);
    }
    const a = 5;
    console.log(a);
    ```
    ```JS
    3
    5
    ```
- functions create a function block
    ```JS
    function demoFunction(){
        const a = 3;
        console.log(a);
    }
    demoFunction();
    const a = 5;
    console.log(a);
    ```
    ```JS
    3
    5
    ```
- `if` statements, `for` loops create a code block
    ```JS
    if (true){
        const a = 3;
        console.log(a);
    }
    const a = 5;
    console.log(a);

    console.log('===');
    for (let i=0; i<1; i++){
        const a = 2;
        console.log(a);
    }
    console.log(a);
    ```
    ```JS
    3
    5
    ===
    2
    5
    ```
- var special case, lifespan extends past block its declared in
    ```JS
    if (true){
        var a = 3; // creates a global variable a
        console.log(a);
    }
    console.log(a);
    ```
    ```JS
    3
    3
    ```
### Hoisting
Function declarations are "hoisted" up to the start of the block. (`let` and `const` are weird here)
```JS
greeting();

function greeting() {
    console.log("Hello!");
}
```
```JS
Hello!
 ```
 Function variables are not
 ```JS
 greeting();

 var greeting = function greeting(){
     console.log("Hello!");
 }
 ```
 ```JS
 TypeError: greeting is not a function
 ```
 Why? Since it is declared with `var` its declaration is hoisted to the start of the block but not its implementation, it does not hold a function when called.

 ### JS base types
 - primitives
 ```
    null // is weird
    undefined
    boolean
    number
    string
    object
    symbol // Added in ES6
```
- Everything starts as `undefined` when declared and not assigned yet.
```JS
let a;
console.log(a);
```
```JS
undefined
```

- `typeof myVar` returns type ( one of the above except `null`) of myVar, `typeof` returns a string
```JS
const a = 42;
console.log(typeof a);
console.log(typeof typeof a);
```
```JS
number
string
```
- why is null weird
    - `typeof null` returns `Object` instead of `null` even though it's one of the base types

- `number`
    - can be integer `42`, float `42.42`, binary `0b1011`, octal `0o377` or hexa `0xFF`
    - can be `Infinity` or `NaN`, both are not actually numbers, but still part of the number type, weird i know
 - `string`
    - used with single, double quotes or backticks
- `boolean`
    - `true` / `false`
    - ### If statements
        ```JS
        if (condition){
            //something
        } else {
            //something else
        }
        ```
        - Has shorthand syntax
        ```JS
        typeof null === "object" ? console.log('Yes') : console.log("No")
        ```
    - ### Truthy falsy
        - `0,- 0, null, NaN, undefined, ""` are all false when used in a logical context
        - anything else is true
        - `==` vs `===`
             - Double equals only compares values (there is casting being done)
             - Triple equals compares both values and types
             ```JS
             const a = 42;
             const b = "42";
             console.log(a == b);
             console.log(a === b);
             ```
             ```JS
             true
             false
             ```

### JS advanced objects
`Array, String, Date, Number, Math, RegExp, Set, Map, Function`
- Each of them has their own methods
- Not enought time to talk about all :<
```JS
const a = [1,2,3,4,5];
console.log(a.indexOf(3));
```
```JS
2
```
---
## Object
- Type that contains data ( even other objects ) and/or functions
- Each key is a string
- Can access with either `myObj.field` or `myObj["field"]`
    ```JS
    const exampleObject = {
        field : "this is a string",
        notTruthy: false,
        exNumber: 42,
        nestedObject: {
            nestedField: null
        }
        myFunc: function(){
            console.log('Hello');
        }
    }
    ```
- `this`
    - special keyword referring to the parent Object of the current executing function, to put it lightly
    - one of the harder concepts in JS
    ```JS
    const exampleObject = {
        name: "Gabriel",
        myFunc: function(){
            console.log(`Hi, my name is ${this.name}`);
        }
    }
    exampleObject.myFunc();
    ```
    ```JS
    Hi, my name is Gabriel
    ```
---
## Functions
```JS
function example1(){
    console.log('ex1');
}
const example2 = () => {
    console.log('ex2');
}
(() => {
    console.log('ex3');
})()
example1();
example2();
```
```JS
ex3
ex1
ex2
```
- Why?
    - Example 3 is an IIFE ( immediately invoked function expression ), creates and executes the function immediately, was used before to contain `var` to not reach the global scope, not much usage nowadays
- What is an arrow function?
    - Example 2 is an arrow function, a short-hand syntax for declaring functions, can be declared with arguments or without, useful in HoF ( high order functions ) for example:
        ```JS
        const a = [1,2,3,4,5,6];
        console.log(
            a.filter( (element) => { return element % 2 === 0})
        )
        ```
        - `filter` is a function of the `Array` native Object, it accepts a function that returns a `boolean`, will loop through the array and will return a copy of the original array with filtered values that returned `true` when applied the given function
        - It can accept 2 arguments if provided, first one (shown here) is the current element of the loop, second one ( not provided here ) is the current index
    - Different examples for writing arrow functions
    ```JS
    const a = () => {/*Do Something*/};

    const b = (arg1) => { return arg1.field};
    const c = arg1 => {return arg1.field};
    const d = arg1 => arg1.field

    const myObj = {
      field: "Hi"
    }
    console.log(b(myObj));
    console.log(c(myObj));
    console.log(d(myObj));
    ```
    ```JS
    Hi
    Hi
    Hi
    ```
    - Note: functions `b, c, d` behave and return the same thing, just written differently, all are valid
---
## Other syntax elements
- Switch cases
    ```JS
    const a = 1;
    switch(a){
        case 1:
            console.log('a is 1');
            break;
        case 2:
            console.log('a is 2');
            break;
        default:
            console.log('a is whatever?');
            break;
    }
    ```
    ```JS
    a is 1
    ```
    ---
    ```JS
    const a = 1;
    switch(a){
        case 1:
            console.log('a is 1');
        case 2:
            console.log('a is 2');
        default:
            console.log('a is whatever?');
    }
    ```
    ```JS
    a is 1
    a is 2
    a is whatever?
    ```
- Spread syntax
    - Definition: It allows an iterable to expand in places where 0+ arguments are expected.
    - Finding max in an array
        ```JS
        const arr = [2, 4, 8, 6, 0];
        const max = Math.max(...arr);
        console.log(max);
        ```
        ```JS
        8
        ```
        ---
    - Inserting into an array
        ```JS
        // Without spread syntax
        const a = [1,2,3,4,5];
        const b = [0, a, 6];
        console.log(b);
        // With spread syntax
        const c = [0, ...a, 6];
        console.log(c);
        ```
        ```JS
        [0,[1,2,3,4,5],6]
        [0,1,2,3,4,5,6]
        ```
        ---
    - Functions with indefinite number of arguments
        ```JS
        function printAll(...args){
            args.forEach((arg) => console.log(arg))
        };

        printAll(1,2,3);
        ```
        ```JS
        1
        2
        3
        ```
- Object destructuring
    ```JS
    const myObj ={
        temperature : 12,
        type: "celsius"
    }
    const {temperature, type} = myObj;
    /*
    const temp = myObj.temperature
    const type = myObj.type
    */
    console.log(temperature);
    console.log(type);
    ```
    ```JS
    12
    celsius
    ```
- Error handling
    - Try catch blocks ( synchronus )
        ```JS
        try{
            const a = false;
            if (a) {
                console.log('a is true');
            } else {
                throw new Error('a is false, abort');
            }
        }catch(e){
            console.error(e)
            console.log('handle it somehow');
        }
        ```
        ```js
        Error: "a is false, abort"
        handle it somehow
        ```
    - Promise rejects (asynchronus )
---
## Asynchronus JavaScript
In my opinion one of the most important things about JS. What happens when we need a part of code or data from somewhere that takes time to run and give us that which we need?

Consider the following example:
- We need to grab some data from an external source, lets say a RESTful API that returns the current temperature
- We can't do something like
    ```JS
    const data = getData('accuweather'); // getData is a placeholder
    console.log(data.temperature);
    ```
    ```JS
    undefined
    ```
- Why?
    - This is because `getData` is an asynchronus method in this example, it takes time for it to get data and return it to our const. Since we made no effort in telling JS this, it executed the function returned nothing ( undefined ) and went on to log it for us
- How can we fix it?
    - Callbacks
        - Callbacks are functions passed as a parameter that execute when called, for example when we receive data from an external source
        ```JS
        console.log('I am here');
        function serverRequest(site, callback){
            // Simulate a delay
            setTimeout(() => {
                const response = {
                temperature: 12
                };
                callback(response);
            },5000);
        }

        function getResults(results){
            console.log("Response from the server: " + results.temperature);
        }

        serverRequest("accuweatherapi", getResults);
        console.log('I am here number 2');

        ```
        - Actual real world example, nodeJS `readFile`
        ```JS
        fs.readFile('path',(err, data) => {
            console.log(`Got data -> ${data}`);
        })
        ```
    - Promises
        - A `Promise` is a native Object that represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.
        ```JS
        console.log('I am here');

        function serverRequest(site){
            return new Promise((resolve,reject) => {
                setTimeout(() => {
                    const response = {
                        temperature: 12
                    };
                    resolve(response);
                },5000)
            })
        }

        serverRequest('accuweatherApi')
            .then((data) => {
                console.log(`Got data -> ${data}`)
            });

        console.log('I am here number 2');
        ```

---
Useful links
- [devdocs.io](https://www.devdocs.io)
- [Mozilla JS tutorial](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics)
- [You don't know JS book](https://github.com/getify/You-Dont-Know-JS)
- [javascript.info](https://javascript.info)