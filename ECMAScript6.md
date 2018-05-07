# Understanding ECMAScript6
## 1. Blocking binding
    - Hoisting: Variable delcarations using var are treated as if they are at the
        top of the function (or in the global scope, if declared outside of a
        function) regardless of where the actual declaration occurs.
    - Block Scopes, also called lexical scopes, are created inside a function or a block.
        let declaration: let value = "blue"  // value will not be hoisted.
        "let" can not redeclare a variable in the same scope.
    - const Declarations: const a = 4; it is block level declaration and not hoisted;
        it cannnot redeclarate variables.
    - Object Declarations with const: prevents modifications of the binding, not of the value.
    - The Temporal Dead Zone: let and const bindings are not accessible before their declarations.
        TDZ: in the scope and before the blocking binding happens. Access a block level variable in
            TDZ throws an error.
        - Block bindings in loops: use "let" to prevent hoisting outside of the loop.
        - Functions in Loops: use IIFE(immediately invoked function expressions) inside loops to
            force a new copy of the variable.
            for (var i=0; i<10; i++) {
                    array.push((function foo(value) {
                        return function () {
                                call(value);
                            }
                    }(i)));
            }
        - let Declarations in loops:
            for (let i=0; i<10; i++)    or  for (let in array or object)
            let creates a new variable i each time through the loop.
        - const Declarations in loops:
            it cannot change loop variables in the normal loop as it tries to increment or decrement loop variable;
            but it can in for-in or for-of loop, as it creates a new binding in each iteration.
        - Global block Binding:
            a new let declaration for RegExp creates a bindings that shadows the global RegExp
                let RegExp = "hi";
                window.RegExp !=== RegExp;
                const ncz = "hi";
                'ncz' in windows; //false
        - Emerging best practices for block bindings:
            Use const by defualt, and only use let when you know a vraible's value to change.'

## 2. Strings and regular expressions
    - character encoding:
        - utf-8: variable-width encoding, backward compatible with ASCII. U+0000 to U+007F take 1 byte,
                code points U+0800 to U+07FF take 2 bytes, code points U+0800 to U+FFFF takes 3 bytes,
                code points U+10000 to U+10FFFF takes 4 bytes. Smallest code unit is 8 bits.
        - utf-16: variable-width encoding. Code points U+0000 to U+FFFF take 2 bytes, U+10000 to U+10FFFF take 4 bytes.
        - utf-32: Fixed-width encoding. All code points take four bytes.
        - Code points: unique identifier for each characters.
    - ES6 support for utf-16:
        - add codePointAt() to access char based on code point instead of char. Each char in ES6 is 16 bits.
        - add String.fromCodePoint() to convert back to string.
        - if working on internationalization, normalize() will be helpful.
    - The regular expression u flag:
        - "u" flag means the regular expression should switch modes to work on characters, not code units.
          /^.$/u.test(text)
            function has RegExpU() {
                try {
                    var pattern = new RegExp(".", "u");
                    return true
                }
                catch (ex) {
                    return false;
                }
            }
    - Other string changes:
        - startsWith, endsWith, includes(); they all take a string. In contrast, indexOf() and lastIndexOf() convert
            a regular expression argument into a string and then search for that string.
        - repeat(): equal to str * number in Python;
   - Other regular expression changes:
        - y Flag: sticky. The global pattern and stick one will keep track of the last match.
                pattern.exec(str), pattern.test(str), pattern.sticky
        - Duplicate regex: new RegExp(re1, "g") overwrites any flag in re1.
        - flags property: re.flags, re.source
    - Template literals:
        to solve multiline strings, basic string formatting, and html escaping.
        let message = `Multiline
                       string`;
        substition: message = `Hello, ${name}`;
        tag_function`template literals`. tag_function(literals, substitutions)
        String.raw`template literals`

## 3. Functions
    ### Default parameters
        - Simulating default parameter vales in ES5:
             function (timeout) {
                    timeout = timeout || 2000;  // buggy: if timeout === 0
                    timeout = (typeof timeout !== "undefined")? timeout : 2000;
            }
        - Default Parameter values in ES6:
            function makeRequest(url, timeout = 2000) {}
            even function makeRe(url, timeout=2000, callback)
            call a function : makeRe("foo/", undefined, callback); // can even set defualt value in the middle parameters.
            call a function : makeRe("foo/", null, callback); // null is considered a valid value in default setting.
        - Default parameter values affect the arguments object:
            in ES5 no strict mode, change to parameters always reflects on arguments array, while not in strict mode.
            In ES6, arguments always reflect initial call state and default value will not be passed to arguments array.
        - Default parameter Expressions:
            function getValeu() {}
            function add ( first, second = getValue())  // it can even be an expression in default setting and be evaluated only when it is called.
            function add(first, second=first); function add(first, second=getValue(first)){}  // can use previous paramaters in the following ones or even in functions.
        - Default parameter TDZ: functional parameters have their own scope and their own TDZ that is separate from the function body scope.
   ### Unamed parameters:
        Javascript donot limit the number of parameter that can be passed to the number of the named paramaters defined. You can pass more or fewer.
        - Rest parameters:
            function pick(object, ...keys) {} // keys becomes an array containing the rest of the parameters. And it doesnot affect a function's length property,
                which indicates the number of named parameters for the function.
            Two restrictions:
                - only one rest parameter
                - the rest parameter must be last.
                - cannot be used in an object literal setter.
            The arguments object always correctly reflects the parameters that were passed into a function call regardless of rest parameter usage.
    ### Increased capabilities of the function constructor:
        var add = new Function("first", "second", "return first + second"); // parameters for the function and function body
        ES6 augments the capabilities of the Function constructor to allow default parameters and rest parameters.
    ### The spread operator:
        ES5: Math.max.apply(Math, valueArray)
        ES6: Math.max(...values); // equal to *array unpacking in python
             Mix and match the spread operator with other arguments as well. e.g., Math.max(...valueArray, 0);
    ### The name property:
        - function.name property
        - clarifying the dual purpose of functions: callable with or without new. When used with new, this value inside a function is a new object
            and that object is returned.
            Javascript has two different internal-only methods for functions: [[Call]] and [[Constructor]]. when called without new, the [[Call]] is called.
            When called with new, [[Construct]] is called and a new object is created and this is set to this object instance..
            Functions that has a [[Construct]] are called constructors.
        - Determing how a function was called in ES5:
                function Person(name) { if (this instaceof Person){} else {throw new Error("You must use new with Person")}}
                To the function, there's no way to distinguish being called with Person.call/apply() with a Person instance from being called with new.
        - The new.target metaProperty
            function Person(name) { if (typeof new.target !== "Person")}  // If [[Call]] is executed, new.target is undefined. And it can be used with a specified constructor.
        - Block-level functions: not allowed in ES5 strict mode.
            Block-level functions are hoisted to the top of the block in which they are defined, but in non-strict mode, they are hoisted all they way to
            the containing function or global environment.
            Use let in function expression will prevent the block-level function hoisting.
   ### Arrow function:
        - No this, super, arguments and new.target bindings. They are definde by the closest containing non-arrow function.
        - Cannot be called with new. it does not have a [[Construct]]
        - No prototype property.
        - Can't change this
        - No arguments object.
        - No duplicate named parameters.
        - Syntax: let sum = value => value; let sum = (num1, num2) => num1 + num2; getName = () => "Qin"; let sum = (num1, num2) => { return num1 + num2;}
        - IIFE: let person = ((name) => { return { getName: function () { return name;}}})("Qin");
        - No this binding: the value of this can change inside a single function depending on the contet in which the funciton is called. It can be fixed using bind(this)
            An array function has no this binding, and it will use the containing no array function's this. e.g. document.addEventListener("click", event => this.dosomething(event.type))
        - Array functions and arrays: values.sort((a,b) => a - b);
        - arguments access: prototype chaining
    ### Tail call optimization: Strict mode
        A tail call is when a function is called as the last statement in another function:
            - The tail call does not require access to variables in the current stack frame.
            - The function making the tail call has no further work to do after th tail call returns.
            - The result of the tail call is returned as the function value. No closure.
        Harness tail call optimization:
            function factorial(n, p=1) {
                if (n<=1) {
                    return n*p;
                } else {
                    let result = p * n;
                    return factorial(n-1, result);
                }
            }


## Expaneded object functionality
    - Object category:
        - Ordinary objects: have all the default internal behavior for objects.
        - Exotic objects: have internal behavior that differs from the default in some way.
        - Standard objects: Defined by ES6, such as Array, Date and so on.
        - Build-in Objects: Present in a JS execution environment when a script begins to execute. All standard objects are built-in objects.
    - Property initializer shorthand: return { name, age };
    - Concise methods: var person = { name: "Qin", sayName() { console.log(this.name);}};
    - Computed property names: property name containing a space can not be referenced by using dot notation. Square brackets allow variables and expression to used inside it.
            var person = { firstname: 'Q', [lastname]: 'C'}
    - New methods:
        - Object.is() works the same as the === operator. but +0 and -0 is not equal, and NaN and NaN are equal
        - Object.assign() might have other names for the same functionality, such as extend() and mix().
            let receivers = Object.assign(supplier, supplier,...)
            Object.assign() doesn't create accessor properties, but only create data propertyies.
    - Duplicate Object literal property allowed: only the last one counts.
    - Own property enumeration order: affects how properties are returned using Object.getOwnPropertyNames() and Reflect.ownKeys and Object.assign(), but not for in loop and Object.keys(), JSON.stringify()
        1. all numeric keys in ascending order.
        2. all string keys in the order in which they were added to the object.
        3. all symbol keys in the order in which they are added to the object.
    - Enhancements for prototypes:
        - allow changing an object's prototype: Object.setPrototypeOf().
            let foo = Object.create(person); Object.getPrototypeOf(foo) === person;
            Object.setPrototypeOf(foo, dog);
          The actual value of an object's prototype is stored in an internal-only property called [[Prototype]]
    - Easy prototype access with super references:
        ES5: Object.getPrototypeOf(this).getGreeting.call(this);
        ES6: super.getGreeting().
             super can only be used in concise methods, otherwise it is an error.
             super references are not dynamic, they always refer to the correct object.
    - A formal method definition:
            [[HomeObject]] references a object which this method belongs to.


## 5. Destructuring for easier data access
    - Object destructuring: let {type, name} = node; // type and name are both declarations of local vars and the properties to read from the object.
    - Destructuring assignment: ({type, name} = node);  // Evaluated to node.
    - Default value: let {type, name, undefind_value} = node;  //undefined_value is assigned to "undefined"
                    let {type, name, value=true} = node;
    - Assigning to different local vars: let {type: localType, name: localName="default"} = node;
    - Nested object destructuring: let {loc: {start: localStart}} = node;

    - Array destructuring: let [first, second] = [1,2,3]; let [,,third] = [1,2,3] // initializer is required when using it with var, let or const.
    - Destructuring assignment: first = 1, second =2; [first, second] = [1,2,3]
        [a, b] = [b, a]  // swap
        [a, b=1] = [2]  // default
        let [first, [second]] = colors;
        [first, ...rest] = [1,2,3,4]  // rest = [2,3,4], rest must be the last item in the destructured array
    - Clone:
        ES5 style: colors.concat();
        ES6 style: let [...clonedcolors] = colors;
    - Destructured parameters: function setCookies(name, value, {secure, path, domain, expires}) // you can use default values, mix object and array patterns, different var names
        Destructured parameters are required, e.g., setCookies('type', 'js') // throws an error.
        function setCookies(name, value, {secure, path, domain, expires} = {}) // make it optional
        function setCookies(name, value, { secure=false, path="/"}={}) // provide default settings
        It makes options object more transparent


## 6. Symbols and symbol propertyies
    primitive types: strings, numbers, Boolean, null, undefined, and symbol.
    - Creating symbols:
        it doesn't have a literal form. let firstName = Symbol(); let person = {}; person[firstName] = "Q"; let lastName = Symbol("last name");
        A symbol's description is stored internally in the [[Description]] property. console.log(lastname) // "last name"
        typeof lastName ==== 'symbol'
    - Using symbols:
        let person = { [firstName]: "Q"}
        Object.defineProperties(person, {[lastName]: {value: "c", writable: false}});
        Object.defineProperty(person, lastName, {writable: false});
        Symbol properties are not enumerable.
    - Sharing symbols:
        let uid = Symbol.for("uid"); // global symbol registry; search first, if no found, create a new symbol, otherwise return existing one.
        Symbol.keyFor(uid) returns key in global registry.
        Symbol("uid") will not put it into global registry.
        Use namespacing of symbol keys like jquery.lastName
    - Type coercion: is prohibited in symbol.
    - Retrieving symbol properties:
        Object.keys() retrieves all enumerable property name;
        Object.getOwnPropertyNames() retrieves all properties, but either of them returns symbol properties.
        Object.getownPropertiesSymbols() gets all symbol properties.
    - Exposing internal operations with well-known symbols:
        - Symbol.hasInstance(): Each function has a Symbol.hasInstance method that determines whether or not a given object is an instance of that function.
        - Symbol.isConcatSpreadable() indicate a object looks like an array: it has a length and numeric keys, and that property values should be added as individuals
            items to an array.
            in concat, arrays are automatically split into their individual items and other types are not.
        - Symbol.mathc, replace, search and split: mimic the followings for custom pattern matchers.
            match(regex), replace(regex, replacement), search(regex), split(regex) in str functions.
    - Symbol.toPrimitive: affect coercion of an object.
    - Symbol.toStringTag:
    - Symbol.unscopables

## 7. Sets and maps:
    in object, the numberic value is converted to a string, so map[5] === map['5']; and object used as a key is also converted into a string.
    for in operator searches the prototype of an object, which makes it safe to use only when an object has a null prototype.
    - Sets in ES6:
        let set = new Set(); set.add(5); set.add("5");
        Sets don't coerce values.
        let newSet = new Set([1,2,3,4,5,6]);  // Set constructor actually accepts any iterable object as an argument.
        set.has(5); set.delete(5); set.clear();
        just as array, forEach(callback, [thisArg]); function callback(parameter in next position, same as first one, ownSet); while in array or map, callback(key, value, collection)
        convert a set to an array: [...new Set(items)];
        ES6 creates weak sets, which only store weak object references and cannot store primitive values. A weak reference to an object doesn't prevent garbage collection if it's the
            only remaining reference.
        let set = new WeakSet();  // prevent memory leak;
        Weak sets aren't iterables and therefore cannot be used in a for-of loop, don't expose any iterators, not have a forEach(), and not have a size property.
        In general, if you only need to track object references, you should use a weak set instead of a regular set.
    - Maps in ES6:
        Map is an ordered list of key-value pairs, where the key and the value can be any type.
        let map = new Map(); map.set("title", "Understanding ES6"); map.get("title");
        as set, map.has(key), map.delete(key), map.clear() and it also has a size property.
        let map = new Map([["name", "Nicholas"], ["age", 25]]); // an array of two arrays;
        map.forEach(callback, [thisArg]); callback(value, key, map); key-value pair in the order in which the3 pair were inserted.
        WeakMap's key must be a non-null object and a value can be of any type. When the key reference is set to null, the weak map has cut off
            access to the value for that key, and when the garbage cllector runs, the memory will be freed.
        WeakMap's methods: get, set, has, delete.
        Use weakMap for private object data.
       This memory management aspect makes weak maps uniquely suited for correlating additional information with objects whose life cycles are managed outside the code accessing them.


## Iterators and generators
    - iterator is an object with a next() method, which returns a object having value and done property.
    - A generator is a function that returns an iterator.
        function *createIterator() { yield 1; yield 2;}
      yield keyword can only be inside generators. Using yield anywhere else is a syntax error, including in functions inside generators.
    - Generator function expressions:
        let createIterator = function *(items) {yield 1;}
        Creating an arrow function that is also a generator is impossible.
        You can even use ES6 shorthand to create generator in object literal.
    - Iterables and for-of loops:
        alll  collection objects(arrays, sets, and maps) and strings are iterables in ES6.
        let values = [ 1,2,3 ];
        for (let num of values) {}
        let iterator = values[Symbol.iterator]()
        for-of loop calls the Symbol.iterator method on the value array to retrieve an iterator.
        Make defined object iterable by creating a Symbol.iterator property containing a generator.
    - Collection iterators: array, set, map
        - entries: key-value pairs
        - values(): values
        - keys(): keys
    - Default iterators for collection types:
        values() for arrays and sets, entries() for maps.
        for ( let [key, value] of mapData)
        WeakSet and WeakMap have no built-in iterators.
    - String iterator:  for (let c of message)
    - NodeList iterator: it can use length and bracket notation, but internally different from array.
        var divs = documents.getElementsByTagname("div");
        for (let div of divs)
    - The spread operator and nonarray iterables:
        array = [...set]
        all = [0, ...s1, ...s3];
        charArray = [...str]
        nodeArray = [...NodeList]
    ### Advanced Iterator function
    - Passing arguments to iterators:
        when you pass an argument to the next() method, that argument becomes the value of the yield statement inside a generator. it is important for asynchronous programming.
            eg. let first = yield 1; iterator.next(1);
    - Throwing Errors in Iterators
        iterator.throw(new Error("Boom"))  // throw() instructs the iterator to continue executing by throwing an error. What happens after that point depends on the code inside the generator.
    - generator return statements
        Use return to exit early and can specify a return value for the last call to the next().
        e.g., yield 1; return 42;
        The spread operator and for-of ignore any value specified by a return statement.
    - Delegate generators:
        function *Iterator3() {
            let result = yield *iterator1();
            yield result;
            yield *iterator2(result);
        }
        You can use yield *str directly on strings.
    - Asynchronous task running
            Instead of needing to use callbacks everywhere, you can set up code that looks synchronous but in fact uses yield to wait for asynchronous operations to complete.


## 9. Introducing Javsscript classes
    - Class-like structure in ES5:
        create a constructor and assign methods to the constructor's prototype.
    - ES6:
        class PersonClass {
            constructor(name) { this.name = name;}
            sayName() { console.log(this.name);}
        }
        class declarations are just syntactic sugar on top of the existing custom type declarations. However, class prototypes, such as PersonType.prototype, are read-only, different from custom type.
    - Why use the class syntax:
        - class declaratoins, unlike function declarations, are not hoisted.
        - All code inside class declarations runs in strict mode automatically.
        - all methods are nonenumerable. while in custome types, you need to use Object.defineProperty() to make a method nonenumerable.
        - All methods lack an internal [[Construct]]
        - Calling the class constructor without new throws an error.
        - Attempting to overwrite the class name within a class method throws an error.
    - Class expression: as well as class declaration, it is not hoisted.
        let PersonClass = class {
            constructor(name) {}
        };
    - Classes as first-class citizens:
        In programming, a first-class citizen is a value that can be passed into a function, returned from a function, and assigned to a variable.
        to create a singleton: let person = new class {}();
        Accesser property: get html(){}; set html(value){};
        It can use computed memeber names: [computedName]
        Define generator: *[Symbol.iterator]() { yield *this.items.value();}
        static member: create members on class instead of prototype. But you can't use static with the constructor method definition.
        inheritance: class Sqaure extends Rectangle { constructor(length) { super(length, length);}}
        You can only use super() in a derived class constructor, call super() before accessing this. The only way to avoid calling super() is to return an object from the class constructor.
        Shadowing class methods: super.method();
        staic memebers are also inherited.
        It can derived from expressions: class Square extends getBase() {}
        Using mixins:
            function mix(...mixins) { var base = function (){}; Object.assign(base.prototype, ...mixins); return base };
            class Foo extends mixin(A, B){}
        Inheriting from built-ins: class MyArray extends Array{}  // length property interacts with numeric properties.
        The Symbol.species property:
        Using new.target in class contructors:
            abstract class:
                class Shape {
                    constructor() { if(new.target == Shape) { throw new Error("This class cannot be instantiated")}}


## 10. Improved array capablities
    - Creating Array:
        Array.of() always creats an array containing its arguments regardless of the number of arguments or the argument types.
        Array.form()s convert an array-like item (numeric index and length property) to an array.
        mapping conversion: Array.from(arguments, (value)=>value+1); or Array.from(arguments, function [,thisArg])
        Use on iterables: let numbers = { *[Symbol.iterator]() { yield 1; yield 2; yield 3;}}; let numbers2 = Array.from(numbers, (value)=>value+1);
    - New methods on all Arrays:
        find(callback[, thisArg]); findIndex(callback[, thisArg]); //usefult to find an array element that matches a condition rather than a value.
        While indexOf() and lastIndexOf() are better for finding a value.
        fill(number, startIndex, endIndex) // endIndex is exclusive. Index can be negative and becomes array.length-number
        copyWithin() Method: copyWithin(destIndex, srcIndex[,length])
    - Typed arrays: 64-bit floating-point, 32-bit integers
        Typed arrays allow for the storage and manipulation of eight different numeric types: int8, uint8, int16, uint16, int32, uint32, float32, float64.
        let buffer = new Arraybuffer(10); //allocate 10 bytes
        buffer.byteLength === 10;
        let view = new DataView(buffer);
        let ints = new Int16Array([1,2]); // length cannot be modified using the length property.
        regular arrays can grow and shrink as you interact with them, but typed arrays always remain the same size.
        typeArray.set() or .subarray()


## 11. Promises and asynchronous programming
    - Promises are another option for asynchronous programming, and they work like futures and deferreds do in other languages.
        Javascript engines are built on the concept of a single-threaded event loop to avoid race condition.
        Javascript engines can execute only one piece of code at a time, so they nee dto keep track of code that is meant to run in a job queue.
        A job is enqued into and dequeued from job queue by event loop. The event loop is a process inside the Javascript engine that monitors code
        execution and manages the job queue.
    - Event Model:
        click/onclick is an event and it adds a new job/event handler to the back of the job queue. And it won't be executed until all other jobs ahead of it are complete.
        Events work well for simple interactions, but chaining multiple seperate asynchronous calls together is more complicated.
    - Callback Pattern:
        Asynchrons_fnc_call(callback(err, data){}); it adds a new job to the end of the job queue with the callback function and its arguemtns. That job executes
        upon completion of all other jobs ahead of it. callback hell: nest too many callbacks.
    - Promise basics:
        A promise ia a placeholder for the result of an asynchronous operation.
        let promise = readFile("example.txt"); //pending state
        - The promise life cycle:
            pending (unsettled); once the asynchronous operation completes, the promise is considered settled and enters Fufilled or Rejected.
            Interal inaccessible [[PromiseState]] property.
            Promise.then(successCallback, failureCallback)
            Any object that implements then() method is called thenable. All promises are thenable, but not vice versa.
            Promise.then(null, function(err)) === Promise.catch(function (err))
            Advantage: events tend not to fire when there's an error, and in callbacks you must always remember to check the error argument.
            Always attach a rejection handler, even if the handler just logs the failure.
            Note: A fulfillment or rejection handler will still be executed even if it is aded to the job queue after the promise is already settled.
    - Creating Unsettled Promises:
        new Promise(executor(resolve, reject){})
        Executor runs immediately when new Promise is called. When either resolve() or reject() is called inside the executor, a job is added to the job queue to resolve
            the promise, called job scheduling. Calling resolve triggers an asynchronous operations. Functions passed to then() and catch() are executed asynchronously,
            because these are also added to the job queue.
    - Creating settled promises
        let promise = Promise.resolve(42)  // create a resolved promise
        let promise = Promise.reject(1)  // create a rejected promise
        promise.then(callback); promise.catch(callback);
    - Non-promise thenables:
        let thenable = {
            then: function (resolve, reject)
                { resolve(45); }
        }
        let p1 = Promise.resolve(thenable); // convert thenable into a fulfilled or rejected promise.
        p1.then(callback);
    - Executor erros: the promise's rejection handler will catch an error thrown inside an executor.
    - Rejection handling: node.js and browser
    - Chaining promises:Promise.then().then(); promise.catch(throw new Error()).catch();
        Awalys have a rejection handler at the end of a promise chain to ensure that you can properly handle any errors that may occur.
    - Returning values in promise chains:
        p.then(function (value) { return value *2}).then(function (value){});
        or use catch(function (value) { return value + 2;}).then(function (value){}) to chain; the failure of one promise can allow the recovery of the entire chain if necessary.
    - Returning Promise in Promise Chains:
       p1.then(function (value){ let p2 = new Promise(function (resolve, reject){}); return p2}).then(function (value){});
    - Responding to multiple promises:
        Promise.all(iterable)  // if all is done successfully, an array of result is returned; otherwise,just the first one failed will return a result.
        Promise.race(iterable)  // depends which one is finished first.
    - support inheritance
    - promise-based asynchronous task running


## 12. proxies and the reflection api
    Arrays are considered exotic objects in ES6.
    Reflect API, trap, proxy
    - creating a simple proxy:
        let target = {};
        let proxy = new Proxy(target, {}); // in this simple example, proxy forwards all operations directly to target.
    - Validating properties using the set trap
        let proxy = new Proxy(target, {set(trapTarget, key, value, reciever) {}})
    - Object shap validation using the get trap
        in JS, reading a non-existent propert doesn't throw an error, and it just returns undefined.
        A object shape is the collection of properties and methods available on the object.
    - Hiding property existence using the has trap.
        foo in something // search the prototype link



## 13. Encapsulating code with modules
    A module is a javascript code that automatically runs in strict mode with
    no way to opt out. Var creatd in the top level of a module are not
    automatically added to the shared global scope.  tow features: this in the top
    level of a module is undefined. Html-style comments are not allowed within
    code.  A module is different from a script.  export anything with name.  import
    {identfier1, identifier2 } from "foo.js" you can't reassign value to imported
    names.  import * as example from "foo.js"; example.add  // namespace import
    import and export can only be used at the top level of a module.  export {sum
    as add}; import {add as sum} from "foo.js"  // export and import rename you can
    only set one default export per module.  export default function (a1, a2) {};
    or export {sum as default}; import sum from "fo.js"  // import the default
    export { sum as add } from "foo.js" // export an imported name import "foo.js"
    // execute the module code without importing any bindings.  imports without
    bindings are most likely to be used to create polyfills and shims.  <script
    type="module" src="module.js"></script> <script type="module">code</script>
    when async used with scripts. it causes the script file to be executed ASAP the file is completely downloaded and parse. However, the order of async
        scripts in the document doesn't affect the order in which the scripts are executed.
    <script type="module" async src="module1.js"></script> same rule as async script.
    CORS: cross origin resource sharing headers
    browser import path: /, ./, ../, url format
    The defer attr is optional for loading scripts files but is always applied for loading module files. The module begins downloading asap but doesn't execute untiil after the doument has been completely downloaded.
