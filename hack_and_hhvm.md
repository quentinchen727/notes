# What are hack and hhvm
Hack is another language with static typechecking, async function, xhp and many more, but it lacks some php's features.
HHVM is an execution engine, suppots both php and hack.
hh_client: typechecker.

PHP is a quite weak and completely dynamic language;
Weak vs Strong or static vs dynamic:
- Can the type be changed during run-time?
- Can the type be coerced into something else easily or implicitly?

You cannot do much without knowing the type of the value, so behind every dynamic language, there is a staticlly typed program hiding.

HHVM version: 3.9, which implements php 5.6 semantics.

 
## Typechecking
Hack adds a layer of robust, sophisticated typechcking to a dynamically typed language.
Hack bypass the compile to detect the type error checking. It uses a client/server model. The typechecking server runs in the background and monitors the filesystem for changes. The client simply queries the servers and displays results quickly, which can be integrated into text editors and IDEs. 
So acutally I can integrate the static typechecking client into vim???

You can use as many or as few typechecking as you want.

.hh_config for all configuration and which files to analyze.
CMD: hh_client

Starting tag: <?hh // strict, while the closing tag ?> is not valid in Hack; PHP's templating-language syntax is not valid in Hack.
Filename exensions are irrelevant.

Autoloading everything:??? "include" or "require" are not mandatory.
Typechecker makes assumption that the project is set up so that any class, function, or constant can be sued from anywhere in the codebase.
PHP uses `Compose` to track and autoload packages. Just one statement in each php file: require 'vendor/autoload.php'
HHVM supports autoloading for classes, functions, and constants in PHP and Hack, plus type alias in Hack.

** Scope in PHP**
Local variable inside functions and global variables are completely isolated.
Two ways to access global vars:
- GLOBALS['foo'], which is a global arrays
- global $foo, to declare foo is global.

function foo($a): int use($parent_var) {} // use to access variables in parental scope. The sequence of use and return type is different from PHP7. _//
Type Annotation Syntax:
1. Function return types: function returns_an_int(): int {}
2. Function parameters: function foo(int a, string b) {} // null is not valid default value in Hack.
3. Variadic functions: PHP function can have more arguments than parameters, while it is an error in Hack. But you can function foo(int a, ...), then use func_get_args() to access arguments.
4. Properties: class foo { public staic string $name; private static int id = 123;}

** Arrays in PHP **
Internally, PHP engine treats arrays with numeric keys and arrays with string keys identically.

Types in Hack:
1. Primitive types(only six): bool, int, float, string, array, and resource. And num(int or float), arraykey(int or string).
2. Object types: class or interface
3. Enums
4. Tuples: tuple($foo1, $foo2). ** You cannot add or remove elements of a
   tuple, but you can change the values, but not the type. is_array(some_tuple)
= true. Type annotation: (int, float). tuple creation: tuple(1, 1.23)
5. mixed: any value
6. void: only valid as a function return value.
7. noreturn: only valid as a return type. The function or method never returns normally.
8. this: only valid as a method return type. It measn the method returns an object of the same class as the object that the method was called on. It is used for method chaining. Class RNG { public function foo($hi): this {return $this;}} // "this" instead of "RNG" solves the subclass problem.
For a static method: class Parent { final protected function __construt(){}  // consturction method
                                    public static function newInstance(): this { return new static();}}
9. Type alias: a way to give a new name to an existing type
10. Shapes: a way to tell the typechecker about the internal structure of array.
11. Type constants
12. Nullable types: All types except "void", "noreturn" and "mixed" can be made nullable by prefixing them with a question mark. e.g., ?int means the value can be an integer or null.
13. classname: classname<Foo> $any_cls_name; then you can use it anywhere a class could be used. Only way to get a classname value is Foo::class, e.g. new, or instanceof, or :: for a static method call. But at runtime, a value of type classname is just a string.
15. Callable types: no "callable" typehint as in PHP; function foo( (function(int, string): string) $callback)
Four ways to call a callable:
- Closure: do(function (int x) {})
- use a named function: do(fun('foo))')
- use a instance method: inst_meth($c, 'do_work')
- use a static method: class_meth(Class_name::class, 'do_work')
- use: meth_caller(Class_name:class, 'do_work'), then call the callable with object and arguments, i.e., $call($obj, arguments)
PHP objects with __invoke() can not be recognized by Hack typechecker.
16. Generics: array<string>, array<string, int>: typechecker knows some is of type X, but not for the runtime.

Typechecker modes:
1. strict: All named functions/methods must have return types and all parameters types annotated, and all proprerties too, except for closure and constructor/desturctor. Restricts: can not call into PHP code; only statements defining named entities can be on top; can not use reference assignment. <?hh //strict
2. partial: does typechekcing, but not require it. Top level code is allowed; can use entities from a Hack file. Reference assigment allowed. <?hh
3. decl: no type check at all. <?hh //del

code without annotations: strict mode hack can call into function(arg_without_annotation), but not product it.

In partial and decl mode, you can call into PHP from hack.
.hhconfig option: assume_php = false/true;

Hack's strict mode doesn't support superglobals: $GLOBALs, $_Session, and etc. But you can write accessor function in a partial-mode file, and call them from strict-mode file.

Overriding methods must have the same parameter types, but can have a more specific return type.
Property initialization: 
1. any non-nullable static property is required to have an initial value, while nullable one is implicitly initialize to null;
2. any non-nullabe non-static property must be initialized in the constructor.
You cannot call public or protected method from constructors before initializing all the properties.

Typed variadic arguments:function sum(SomeClass ...$args). Typechecker supports it, while Hack does not support annotated variadic parameters.

Types for Generators:
1. Iterator<int>
2. KeyedIterator<int, string>
3. Generator<int, string, User> used with send()  // something similar with python async call

Fallthrough: put "// FALLTHROUGH" as the last line of the falling-through case, except it is empty;

Type inference:
local vars are not declared, and can be reassigned to hold new type of value.
unresolved type.

Redefining types:
is_string(), ===, !==, invariant(), instanceof

## Generics
class, function, method, trait, type alias, and interface can all use generics.

Type Erasure: HHVM is almost completely unaware of generics. So you cannot:
1. new T()
2. T::foo() or T::Foo
3. function f<T>(T<mixed> $value)
4. instance of T
5. (T) $value
6. catch block

Adding constraints to generic, like, <Tval as num>, adds more legal operations to the type parameters.
Arrays and immutable Collections support subtype matching.
Array has pass-by-value semantics.

## Enum
enum CardSuit: int (or string, the underlying type) {
    A = 0;
    B = 0;
}
Enums are distinct types.
To convert a value of enum type to its underlying type, just use a regular PHP
cast expression: (int) or others.
if enum CardSuit: int as int{} , you can implicitly convert it to int.
One benefits of enums over class constants is that when a value of enum type is
used as teh controlling expression of a switch statement, the typechecker can
ensure that all cases are handled, but default will silience that.
Enums act like psudoclasses, share classes' namespace. But it has six static
methods: assert, assertAll, coerce, getNames, getVallues, isValid.

type some = Map<int, string>; // trasparent; cannot be converted
newtype userid = int; //opaque; can be converted
Both declaration must be at the top.

In general, you can only use type alias in type annotation.

Type constant and type constraint

Array Shapes declaration: shape('id'=>int, 'name'=>string)
A shape is really just an array with special tracking by the typechecker.
structural subtyping

Shapes is an abstraxct final class that contains: idx, keyExists, removeKey, toArray.
Restrictions on Shapes:
1. can't read or write with unknown keys.
2. can't use append operator.
3. don't implement Traversal or Container.

lambda expression: ==>, it can automatially capture enclosing scope.
while in php, you have to capture the enclosing variables manually.

Constructor parameter promotion: __construct(private int id = 1, pubic name =
'foo')

Attributes: <<someString>>, then use ReflectionFunction/Class/Method to access them.
Special attibutes: __Override, __ConsistentConstruct, __Memoize(specially by HHVM)

Integer arithmetic overflow will produce a float.

Nullsafe method call and property access: $reader?->readAll(); // return a
nullable version of the actuall method's return type.

It is not possbile to instantiate a Trait on its own.
overwrite: current members >> trait methods >> inherited methods
U can use multiple traits, seperated by commas.
Two traits inserting a method with the same name is a conflict, and you can
reslove it by using insteadof operator:
use A, B {
    B::smallTalk insteadof A;
    A::bigTalk insteadof B;
    B::bigTalk as talk;
}
change trait visibility:
use HelloWorld { sayHello as protected; }
You can compose traits from traits.
trait HelloWorld {
    use Hello, World;
}
Abstract trait members:
trait Hello {
    abtract public function getWorld();
}
Static method and member.
Each class using a trait has its own static members.

Essentially, a trait is language assited copy and paste.
A trait is essentially a bundle of code taken out of context. Traits must be
able to refer to properties and method that they don't define.
trait has Require {
    require extends C;
    require implements I;
}
A class must extend or implement in order to use a trait.

Silence typechecker errors: /* HH_FIXME[error_code] some other comments */
/* HH_IGNORE_ERROR[error_code] some other comments */ is put before the function or statements.

## PHP features not built into Hack
1. Reference: "action at a distance"
2. the global statement
3. Top-level code. Top-level code is not allowed in strict mode, only in partial mode.
4. Old style constructor
5. case-insensitive function and class name lookup
6. Variable variables
7. Dynamic properies
8. Mixing method call syntax: static/non-static
9. isset, empty and unset: use array_key_exists() instead.

## Collections built in Hack
Vector, Map, Set, Pair, ImmVector, ImmMap, ImmSet.

Arrays have `value` semantics, whereas collections have reference semantics.
Arrays has copy-on-write, which is like a bug in the language.

literal syntax:
Vector{1,2,3}; Map{'a'=>1, 'b'=> 2};

Use collection in PHP: HH\Vector 

Adding or remvoing an item in a collection while iterating over it with foreach
is not allowed.  [] could add value in collection; removeKey(), remove()
remove.
list assignment.

Core interfaces
General Collection interfaces
Specific Collection interfaces
Concrete Collection Classed
Conversion to array
var_dump()
implicit conversion

## Async
Like PHP, Hack doesn't support multithreading.
Async offers way to implement cooperative multitasking, compared to preemptive
multitasking, in which tasks are forcibly interrupted by the task manager.
No critical section is cooperative multitasking.

Methods, both static and non-static, and closure can be async.

First, async functions don't necessarily need to be inherently asynchronous at
all. A call to an async function returns an object that represents a result
that may or may not be ready.  Second, async functions have a special return
type, an object that implements the interface `Awaitable` as the return
annotation say. The type argument to Awaitable is not erased at runtime.

Use await to get the value out of the Awaitable object: await Awaitable<T>

Async is different from Ajax, or Promise, or Future in that it will suspend the
ccurrent execution, saves ints execution state, and picks up execution
somewhere else, and once the result is ready, it can resume the execution
again. In Future, it will continue execution of the current routine, and will
only jump to "callback" function once the promise is fulfilled.

Another benefit from async is to swait multiple asynchronous operations at the
same time, e.g., await HH\Asio\v(array(fetch_from_web(), fetch_from_db())).

Async: install asio-utilies, a library of async help functions. Use Composer to
download it and add it to composer.json file.  Exceptions in Async functions
and Wrapper in asio

HH\Asio\v turns a vector or array of awaitable into an awaitable Vector.
Likewise, HH\Asio\m() turn a Map into a avaiable Map.
For a non-async function to get a result out of a wiat handle, there is
HH\Asio\join() to use on an Awaitable. But it will block all other in-flight
awaitables. Don't use it.

 You cann't declare abstract methods, or methods in interafce, or callable to be
async, but you can declare them as non-async, but with Awaitable as their
return type.

Await is not an expression. Await can be used only in its own expression,
assignment or return statement.

Async generators:
AsyncIterator<int>
foreach($async_gen await as $value) {}; a shorthand for await $async_gen->next()
AsyncKeyIterator<int, string> for key.

If u want to call send() or raise() on an async generator, you need to use the
AsyncGenerator instead. It has 3 arguments: key type, value type, and the type
you want to pass to send().
It is always valid to pass null to send(). await $foo->send($id) might return
null. when calling next, send, raise on async generator, you have to await
them, not just call them.

$handler = thrower(); // get a handler
await $handler; // get the result
If one of the component wait handles throws an exception, the combined wait
handl will rethrow that exception. If multiple component wait handles throw
exceptions, the combined one will rethrow one of them. However, all the
component wait handles will complete.
Use HH\Asio\wrap() to wrap a wait handle. It returns a ResultOrExceptionWrapper
which has isSucceeded, isFailed, getResult, getException.

Mapping and filtering helpers
1. The first character is v(vector) or m(map), indicating what the function
   returns;
2. next, you might see m, mk, f, or fk. m and mk mean mapping; f and fk
   callback. k means the key from the collection will also be passed to the
callback as well.
3. w means wrapper.
The first argument is always the input array or collection, but it can be
Traversable or KeyedTraversable.
The mapping and filtering callbacks are 'async' functions. Mapping callbacks can
takes one parameter, of the value type, or two parameters, of key and value,
and returns any type. Filtering can only return booleans.

Data dependencies:

Antipatterns: creates false dependencies.
1. await in the loop
You should use one of the await-a-collection helpers instead of iterating over
the collection and awaiting in a loop.
2. multi-id pattern
Don't create false dependencies.

Other types of waiting:
1. sleeping
HH\Asio\usleep(100000) is akin to calling to usleep(), except that it allows
other wait handlers to run during the sleep period.
2. rescheduling: reason to do this: polling, and batching.
Send it to the back of the async scheduler's queue.
polling: while (!svc_isReady()) { await HH\Asio\later();} // if there is no
other wait handler running, this amounts to a busy loop of polling.

Awaiting a wait handler guarantees that it will run to completion.
Async doesn't provide a way to detach tasks.
The creation and awaiting of a wait handle should happen as close together as
possible.

Async doesn't create threads. HHVM does run multiple web requests in parallel
using system-level threads, but the PHP/Hack environments in those threads
can't substantially interact with each other.
Hack is not the right language for cpu-intensive work.

3. Memoizing async function
The correct solution is to memoize the wait handle, not the result.
```
    static $handle = null;
    if ($handle == null) {
        $handle = time_consuming_op_impl();
    }
    return await $handle;
__Memoize attribute

```

Async extension:
1. queries to MySQL: network request
AsyncMysqlClient, connect, query, queryf,
AsyncMysqlConnectionPool for multiple connections.

2. queries to memcached: network request
MCRouter is a memcached protocol routing library, providing a wide variety of
features that aid in scaling a memcached deployment.

3. cURL request: network request
curl_multi_await()
HH\Asio\curl_exec()

4. reads and writes of Streams sources: IO request
steam_await(reousrce $fp, int $events, float $timeout = 0.0) :Awaitable<int>;

Vector{1,2,3}
Map {"a"=> 1, "bc"=>2}
## XHP
XHP is a feature of Hack that allows programmers to represent an HTML tree as
PHP/Hack objects. The HTML-like syntax is part of the grammar.
XHP is a great foundation for a modern, object-oriented web app UI library.

Benefits:
1. runtime error detection.
    XHP can check html constraints and raise erros if they're violated.
2. Secure by default
no calls to htmlspecialchars() is needed.
XHP gives you a way to build up a structure using PHP/Hack objects and then
serialzied it to HTML, without dealing with a serialized HTML string except to
output it into a stream.

HHVM has support for XHP built in.
Need the Hack library for XHP: 'facebook/xhp-lib':"~2.2", by using Compose.
This conatins classes that form the infrastructure of XHP, as well as classes
that mirror all the tags that HTML5 supports.

Instead of creating XHP objects with the keyword `new`, you create them with
XHP tags.
XHP is syntatic sugar for creating XHP objects.
:xhp, XHP classes starts with a colon (:), and they descend from the core XHP
library class :xhp.
XHP objects are instances of XHP classes. Each object can have any number of
children, either text or another XHP object.
In text within a XHP, whitespace is mostly insignificant.

Basic tag usage:
html character reference.
attributes. Each XHP class defines the attributes that it can have, and each
attributes has a type, and optionally, a default value. Attribute values are
not subject to variable interpolation.
Embedding Hack code: using {}

Type annotation for XHP: XHPRoot and XHPChild
XHPRoot is any object that is an instance of an XHP class; XHP is the set of
things that are valid as the value of $xhpchild in <div>{$xhpchild}</div>;
x:frag is a transparent wrapper.

Object interface
Validation

Creating XHP Classes: 
XHP class names starts with a colon(:), and may includ hypens, returns an XHP
object.
class :hello-wolrd extends :x:element{ 
    attribute int proflieid @required;
    protected function render() :XHPRoot
}
use $this->:name to acesss its attributes.
Attribute types: 
- bool, int, float, string, array, and mixed.
- enum;
- class and interface names: specially, Stringish, implemented by string and
  any class with a __toString() method.

attributes: attribute bool foo = false; // optionally a default value

Inheriting attributes:
class :ui:drop-foo extends :x:element {
    attribute :div;
} // this only declares attributes; it doesn't include any automatic transfer
of :idv attributes of :div to the div it generated. So use XHPHelpers traits to
get automatic transfer.

Children declaration:
If there is no children declaration, the class is allowed to have any number of
children fo any type.
children emtpy; // no children allowed
children (:head, :body); // required to have a <head> and <body> child in that
order.
children (pcdata)*;

Category: 
category %flow, %phrase; // similar to interface in oop.

Context:
$father->setContext('foo', 1);
$child->getContext('foo');
Context is only passed down the tree at render time.
Parent does not overwrite children's key.

Async XHP:
1. use trait XHPAsnyc;
2. implement the function asyncRender() instead.

XHP Helpers:
use XHPHelpers;
1. transferring attributes. XHPHelper will transfer each attributes declared on
   the xhp class, except for class attributes, which will be appended..
2. unique IDs. $this->getID()
3. Managing the class attribute.

XHP Best practices:
100% of FB's web fronted code uses XHP to generate HTML.
1. [XHP-Boostrap]
2. No additional public api: only public apis are attribute and children
   declarations
3. Composition, no Inheritance.
4. Don't make control flow tags.
5. Distinguish attributes from children
6. style guide:
        - separate words in XHP class names with hyphens. All lowcases.
        - use colons in XHP class names as a form of namespacing. However,
          there are no real namespacing semantics.
        - only have the attribute keyword once.

## Confiugring and deploying hhvm
HHVM uses OS-level threads, unlike PHP-FPM, which uses processes. The overhead
of increasing HHVM's thread count is quite low.

Server Mode:
vs command-line mode.
Run a FastCGI server, and hook it up with a web server like Apache or nginx.

Warming up the JIT
Admin server

## hphpd: interactive debugging
local mode for using hphpd as a REPL or debugging a script.
Remote mode for debuggig web apps.

## Hack tools
The core of the Hack typechecker's infrastructure is a server that remembers a
set of facts of the codebase. 
use hh_client to check type errors.
hh_client --search, search-class, search-functions, search-constant,
search-typedef.
--find-res: given function or method
--find-class-res: given class.
--inheritance-ancestors, --inheritance-children.

--json to produce the output in JSON.

** Hack arrays
History Hack collections: Vector, Map and Set. Good for compatiblity with php array
functions, and adding other utility functions and types. Bad for `reference
semantics` and slowness.

Hack Arrays: `vec`, `dict`, and `keyset`. They are best. Value semantics, solve
typing, can use with php library functions plus new HSL, no key coercion, and
accessing unset keys will throw. Fast!!!
vec[1,2,3] //literal;
vec(array(1,2,3)) // from another container
$v[]=4; // append
vec<int> $v;
dict['foo'=>'bar']; dict(array('foo'=>'bar'));
$d['foo'] = '112';
keyset['foo', 'bar'];
keyset(array('1','2'));
$k[]='baz';
C\contains_key($k, 'foo');
NOTE: Prefer Hack arrays(vec, dict, keyset) over Hack Collections(Vector, Map,
Set) and PHP (array) whenever possible.

## Trait in PHP
It is about horizontal composition. It will avoid the typical problems
associated with multiple inheritance and Mixins.
* Precedence *
Current method overrides trait, and trait overwrites inherited.
* Conflict Resolution *
If two traits have the same names, use 'insteadof' to solve the conflict.
    use A, B {
        B::smallTalk insteadof A;
        A::bigTalk insteadof B;
}
* Change method visibility *
use A { smalltalk as priavte mySmalltalk; }
* Compose a trait from traits *
* Abstract trait members *
Abstract methods of the trait imposes requriements upon the exhibiting class.
* Traits can define both static members and static methods *
* Properties *
If a trait defines a property then a class cannot define a property with the
same name unless it is compatiable(some visibility and initial value),
otherwise a fatal error is raised.
