# data type in C++ float and double are stored in scientific notation: sign
digit + matissen(1.12324) + exponent(-126....+127); it is using an unsigned
char + bias to represent the exponent, and the middle is 127(bias) to represent
zero, so the minimum is 1 - 127 = -126.  char is either signed char or unsigned
char, depending on the compiler.

Type conversion: non 0 value is true bool.  true bool is 1, while false is 0.
a float will be truncated to a int.  a integer to float: its precision will be
lost. Because only maybe 7 bits is the matissen.  a out-of-range value to an
unsigned type, the result is the remainder of the value modulo the number of
values the target type can hold.  an out-of-range value to an object of signed
value, the result is undefined.

FQA:
1. what is indirect reference?
A variable directly references a value, while a pointer indirectly references a
value through the memory address it stores. * is called dereference operator or
indirection operator.

**Advice**: avoid undefined and implemention-defined behavior.  Undefined
behavior results from erros that the compiler is not required (and sometimes is
not be able) to detect.

Don't mix signed and unsigned types. The signed values are automatically
converted to unsigned.

Integer and Floating-point literals Prefix: u, unicode 16 characters, char16_t
U, L, u8 Suffix: u or U, unsigned, l or L, long ll or LL, long long f or F,
float l or L, long double

The type of a string literal is array of constants chars. The compiler appends
a null char '\0' to every string literal. Two string literals adjacent to each
other and seperated only by white space characters are concatenated into a
single literal.

\escape sequence like \n, \t, or \octal, \xHexadecimal
\Octal_use_at_most_3bits, \Hexadecimal_use_all_digits

The word `nullptr` is a pointer literal.  true/false

initialization != assignment

list initialization int foo =0; int foo = {9}; int foo{9}; int foo(0);

**Initialization** The variables defined outside any function body are
initialized to zero, while undefined in function body.  Some classes require
that every object be explictily initialized.

seperate compilation

An uninitialized variable has an indeterminate value. We'd better initialize
every object of built-in type.

A declaration makes a name known to the program.  A definition specifies the
type and name of a variable, and it is also a declaration, alloating storage
and may providing the variable with an initial value.  extern int i; //
declaration but not definition int i; //declare and define i; extern int i = 1;
// override extern and it becomes definition It is an error to provide an
initializer on an extern inside a function.

C++ is a statically typed language, which means that types are checked at
compile time.

global scope vs block scope

It is usually a good idea to define an object near the point where the object
is first used.  Scope shadowing: use ::global_var to access global var.

**Reference** A reference defines an `alternative` name for an object. We bind
the reference to its initializer. There is no way to rebind a reference to
refer to a different object. And references must always be initialized.  A
reference is just an `alias`.

A compound type is a type that is defined in terms of another type. Two of them
are references and pointers.  int &d = foo; A reference only binds to an
object, not to a literal or to the result of a more general expression.

int *d; // a pointer is an object in its own right. * is part of declaration.
&: address of; *: dereference, an operator.

Null pointer: int *p1 = nullptr; //base type + type modification.  int *p1 = 0;
int *p1 = NULL; // a preprocessor variable.

void *d; // use a void* pointer to deal with memory as memory, rather than
using the pointer to access the object stored in that memory. void * can hold
any type.

int *&r = p; // read from right to left.

const int i = get_size(); // initialized at run time;
const int j = 42; // initialized at compile time;
const int k; // error: k is not initialized.
const int foo = 12; // must be initialized.  The compiler will replace uses of
the const variable with its corresponding value during compilation.  

**Note**
const int * c1 is equal to int const * c1;
int * const c1 is a constant pointer to int.

**By Default, const objects are local to a file.  If you want to share across
files, use: extern const int buf = 12; then declare: extern const int buf in
head file. We must use the keyword extern on both its definition and
declaration.
 Reference binding cannot be changed. const Reference is a Reference
to const.

**Pointers and references to const think they point or refer to const.  const
int *p1 = &p2 // low-level const

const pointers: int const *p1 = p2; // top-level const const double *const pip
= &pi; // a const pointer to a const double

**NOTE of casting**
1. static_cast<type> is useful when a large arithmetic type is assigned to a
   smaller type. This can inform reader and the complier, and turn off the
warning message.
doubl slope = static_cast(j)/i;
void *p = &d;
double *dp = static_cast<double*>(p);

2. const_cast
it changes only a low-level const in its operand;
const char *pc;
char *p = const_cast<char*>(pc); // The compiler will not prevent us from
writing to that object. However, using a const_cast in order to write a const
object is undefined.

const int* const Foo(const int* const &) const;

A constant expression is an expression whose value cannot change and that can
be evaluated at compile time.  constexpr int mf = size(); // ask compiler to
verify the expression to be constant.  constexpr int *q = nullptr; // q is a
const pointer to int.

type alias: typedef double wages; or using SI = SaleItem;

Using auto to let the compiler figure out the type for us.  auto foo = f1 + f2;
// must have an initializer; vars in one line must have consistent types.  But
must explicitly specify: const auto f = ci;

decltype(f()) a = b; // a has whatever type f returns, but is bound to b;
decltype((var)) is always a reference type, but dectltype(var) is not always.

The library type string, istream and ostream are all defined as classes.
struct foo{...}; foo a, b, *c;

In-classes initializer: member without a in-class initializer will be default.

C plus scope: global, class, namespace, block

Vector has variable length.

using namespace::name; // namespace using declaration

Headers should not include using declarations, as it may introduce conflicts.

Include string to perform required operation for string type.

We can define class inside or outside a function just as in Python.

In order to ensure that the class definition is the same in each file, classes
are usually defined in header files.

User Preprocessor guards to prevent multiple header file inclusion.

String initialization: string s2(s1); string s3("value"), string s3 = "value";
string s4(n, 'a') support operations for string: <<, >>, getline, empty(),
size(), [], +, =(copy), ==, !=, < <=, >, >=

size() return string::size_type, a companion type which makes it possible to
use the library types in a machine-independent manner.  
*****Mixing unsigned and signed data can have surprising effects. negative int
will be converted to a large unsigned: int -> unsigned.

String literals and strings are different.  So "hello" + ", " is not allowed,
while s1 + "hi" is OK.  The string library convert both character literals and
character string literals.


cctype headers: isalnum, isalpha, iscntrl, isdigit, isspace,....


the C++ library incoporate the C libary, and it removes the .h suffix and
precede the name with the letter c.  for (auto c : str) range operator. // the
next character will be `copied` into c.  for(auto &c : str) // can use
reference to modify in place.  if s is empty, s[0] is undefined.

C string is just char*, different from C plus string, a class object.  The
result of using an out-of-range subscript is undefined.

#include <vector> using std::vector; A vector is a class template. C++ has both
class and function templates.  Templates are not themselves functions or
classes. They can be thought of as instructions to the compiler for generating
classes or functions. The process that the compiler uses to create classes or
functions from templates is called `instantiation`.  vector<int> ivec;
//instantiation

Vector initialization: vector<T> v1; // v1 is empty vector<T> v2(v1) // v2 has
a copy of each element in v1 vector<T> v2 = v1 // ditto vector<T> v4(n) // v4
has n copy of value-initialized object vector<T>v5{a,b,c..} // initialize by
order.  vector<T>v6={a,b,c...} // ditto

vector<int> ivect(10) // ten elements, each initialized to 0 vecotr<int>
ivecstr(10, "a") // ten elements, each to "a"

List initializer or element count; v2.push_back(i); // add element The body of
a range for must not change the size of the sequence over which it is
iterating.  vector operations: empty(), size(), push_back(), [], =, {}, ==, !=,
< <=,>,>=

Iterator gives us indirect access to an object. A valid iterator either denotes
an element or denotes a position one past the last element in a container.
auto b = v.begin(), e = v.end(); // b and e has the same type. e iterator is
off-the-end iterator, or the end iterator.

iterator operation: *iter: return a reference to the element denoted by the
iter.  iter->mem: Dereference iter and fetch the member mem ++, 00, ==, !=

Only a few library types, vector and string being among them, have the
subscript operator.

vector<int>::iterator it; string::const_iterator it2;

auto it = v.cbegin(); // const_iterator.  The loops that use iterators should
not add elements to the container to which the iterator refer.  Iterators are
somehow like pointers.

Arrays are fixed sized, but they have run-time advantage. As a result, the
dimension must be known at compile time, which means that the dimension must be
a constant expression.  We cannot initialized an array as a copy of another
array, nor it is legal to assign one array to another.

size of an array is of type size_t, which is defined in the cstddef(c+) or
stddef.h(c).

pointers and arrays are closely intertwined.  #include <iterator> int *pbeg =
begin(arr), *pend = end(arr); auto n = end(attr) - begin(attr); // the result
is a `ptrdiff_t` type.  int k = p[-2]; // unlike subscripts for vector and
string, the index of the built-in subscripts operator is not an unsigned type.

C-style character strings are null terminated and stored in character arrays.
#include <cstring> // c-style strings are error-prone.

const char *str = s.c_str(); // convert to a c style string.

Use an array to initialize a vector

**Use library types instead of arrays** 
Use vectors and iterators instead of built-in arrays and pointers, and use
strings rather than C-style array-based c-string.

To use a multidimensional array in a rane `for`, the loop control variable for
all but the innermost array must be references.

there is no order of evaluation, like f() * g();// f or g first??

Overflow and arithmetic exceptions will produce undefined results.

m%n has the same sign as m assignment has the lower precedence

The prefix version ++ avoids unnecessary work.

Statements: Most statements end with a semicolon.  ; //null statement compound
statement; empty block;

If we need to defind and initialize a variavble for a particular case, we do so
by defining the var inside a {} block.

C++ exception will propragate along the function chain, like in python. If no
one is caught, execution is transferred to a library function named
`terminate`.

C++: Writing exception safe code is Hard.  `exception` header: the most general
kind `stdexcept`: several general-purpose exception classes `new`: the
bad_alloc exception type `type_info`: bad_case type Provide initializer to
provide additional information about the error that occurred.

Parameters and var defined inside function are local, associated with automatic
objects.  static int c; // local static object is initialized before the first
execution, and is only destroyed when program is done.

Function declarations go in header files.  Passing auguments by value: foo(int
i); Passing auguments by reference: foo(int &i);

Using reference parameters to return additional information Use reference to
const when possible.

Because arrays are passed as pointers, functions donot know th size of the
array they are given.

int main(int argc, char **argv); int main(int argc, char *argv); argv[0] is the
name of main function.

All arguments have the same type: initializer_list<>; ELLIPSIS parameters:
foo(...) // use a c library name varargs

values: the return value is used to initialized a temporary at the call site,
and that temporary is the result of the function call.  Do not return a
reference to a local var, as the local var will be freed after the function
call.

list initializing the return value: return {'a', 'b', 'c'};

cstdlib: EXIT_FAILURE; EXIT_SUCCESS; // preprocessor

int (*func(int)) [10]; auto func(int i) -> int(*) [10]; // declare a function
that returns a pointer to an array.

Overload functions parameter names in function declarations are optional.

In C++, name lookup happens before type checking.  It is a bad idea to declare
a function locally.  If we declare a name in an inner scope, that name hides
uses of that name declared in an outer scope. Names do not overload across
scopes.

###Functions with default arguments.  Default arguments should be specified
with the function declaration in an appropriate header. 
Names used as default arguments are resolved in the scope of the function
declaration, which means the memory location for that parameter name is already
fixed when declared.
But the value is evaluated at the time of the call.  It is normal practice to
declare a function code inside a header, and it is legal to redeclare a
function multiple times.  But, any subsequent declaration can add a default
only for a parameter that not previously had add default specified. As usual,
defaults can be specified only if all parameters to the right already have
defaults.

The inline specification is only a request to the compiler. The compiler may
choose to ignore this request.  In general, the inline mechanism is meant to
optimize small, straight-line functions that are called frequently.

A `constant expression` is an expression whose value CANNOT change and that can
be EVALUATED at COMPLIE time.  A `const` object that is initialized from a
constant expression is a const expression; a literal is a constant expression.
In order to be able to expand the function immediately, constexpr is implicitly
inline. A constrexpr must take a literal parameter. The constexpr function is
defined like any other functions but meet certain restrictions: the return type
and the type of each parameter must be a literal type, and the function body
must contain exactly one return statement.

The arithmetic, reference,and pointer types are literal types. our Sales_item
class, the library IO and string types are not literal types. Hence, we cannot
define variavbles of these types as constexpres.  Put inline and constexpr
functions in Header files.

assert is a preprocessor macro, defined in the cassert header.  NDEVBUG to turn
off assert. CC -D NDEBUG to turn off assertion.  __func__, __FILE__, __LINE__,
__DATE__, __TIME__

Casts should not be needed to call an overloaded function.

declare a function pointer: bool (*pf) (const string &, const string&); pf =
foo; equal to : pf = &foo; pf(a,b) equal to (*pf)(a,b)

int (*f1(int))(int*, int) // f1 return a function pointer.

class: this is implicitly passed into memeber function. And it is unnecessary
to access the member var via this. `this` is a pointer.

std::string isbn() const {return this-bookNo;} // const member function cannot
write, only read. const member function may call only const member function.

All member funcs must be declared inside class; We can declare a member
function iside the class, then we can Define member function outside the class:
Foo::abc() const {}

Sale_data& combine(const Sale_data &rhs) { return *this;} defining a member
function to return "this" object: return *this;

A class itself is a scope.  The compiler processes classes in two steps: the
member declaration are compiled first, after which the member function bodies,
if any, are processed.  The only difference between using class and using
struct to define a class is the default access level. class: private; struct:
public;

A class can allow another class or function to access its nonpublic memebers by
making that class or function a friend, by adding a friend keyword.

If no explicitly defined constructors, the compiler will implicitly define the
default constructor without parameters for us. This is known as `synthesized
default constructor`.

Explicit default constructor: Sales_data() = default // take no parameters.The
compiler generates the default one for us, and the data memembers will use in
class initializer value.  Constructor initializer list: Sales_data(const
std::string &s): bookNo(s){}

Construcotrs should not override in-class initalizer except to use a different
initial value.

Constructor Initializer List: Sales_data(cost std::string &s) : bookNo(s) { }
// a list of member names, fllowed by the initial value. When a member is
ommitted from the constructor initializer list, it is implicityly initialized
useing the same process as by the synthesized default constructor. These
constructors could have empty function bodies.

Defining a constructor outside the class body.  Sales_data::Sales_data(){}

Copy, assignment, and destruction Object is data stored in memory. Objects are
copies in several contexts, such as when we initialized a variable or when we
pass or return an object by value. Objects are assigned when we use the '='.
Objects are destroyed when they cease to exit, such as when a local object is
destroyed on exit from the block in which it was created. Objects stored in a
vector/array are destoryed when that container is destroyed. If we do not
define these operations, the compiler will synthesize them for us.  However,
the synthesized versions are unlikely to work correctly for classes that
allocated resources that reside outside the class objects.  The synthesized
version for copy, assignment, and destruction work correctly for classes that
have vector or string members. The vector class takes care of copying or
assignment or destroying the elmemnts in that member.

Usually, the resources your classes allocate should be stored directly as data
members of the class.

##7.2 Access Control and Encapsulation public: and private: There are no
restrictions on how often an access specifier may appear.

If using the `struct` keyword, the members deined before the first accdess
specifier are public; if `class`, then private.  The only difference between
using class and struct to define a class is the default access level.

A class can allow another class or function to access its nonpublic memembers
by making the class or function a "friend". Precede that fnc by the keyword
"friend".

By defining the data memember as "private", the class author is free to make
changes in the data, and it will ease the problem of maintenance and program
correctness.  Although user code need not change when a class definition
changes, the source files that use a class must be recompiled any time the
class changes.

##Additional class feature
1. defining a type member: class Screen { public: typedef
   std::string::size_type pos; } // type members are subject to the same access
control.  or using pos = std::string::size_type; // members that define types
must appear before they are used.

implicit and explicit member functions of class. We can declare or define
incline functions.

A "mutable" data member is never const, even when it is a member of a const
object. Accordingly, a const member function may change a mutable member.
mutable size_t access_ctr;

When we provide an in-class initializer, we must do so following an = sign or
inside braces.

Function that return *this: inline Screen &Screen::set(pos r, pos col, charch)
{ return *this;}

A const member function that return *this as a reference should have a return
type that is a reference to const.

class declaration: we can declare a class without defining it.  A class must be
defined before we can write code that creates objects of taht type. Similarly,
the class must be defined before a reference or pointer is used to access a
member of the type. So we can not define a class having data member of its
type, but it can have pointer member pointing to its own type.  friend class
Windows_mgr; The member functions of a friend class can access all the members,
including the nonpublic members, of the class granting friendship.  We can just
make a member function a friend.

A friend declaration affects access but is not a declaration in an ordinary
sense.

Class scope: We access type member from the class using the scope operator ::.
Ordinary data and function members may be accessed only through an object, a
reference, or a poiter using a member access operator.

Once the class name is seen, the remainder of the definition is in the scope of
the class. On the other hand, the return type of a function appears before the
function's name, so we need to use a scope operator to access class type.

## Name Lookup and Class Scope Definitions of type names usually should appear
at the beginning of a class. That way any membe that uses that type will be
seen after the type name has aleady been defined. It is an error to redefine a
type name.

Use ::height to access the global variable.

## Constructors If we do not explicitly intialize a member in the constructor
initializer list, that member is default initialized before the constructor
body starts executing, which will slow the program.  So it is better to have
initializer list.

Members that are const or reference must be initialized.  We must use the
construcrtor initializer list to provide values for members that are const,
reference, or of a class type that does not have a default constructor.

It is a good idea to write constructor initializers in the same orders as the
members are declared. Moreover, when possible, avoid using members to
initialize other members.

Order of member Initialization: Members are initialized in the order in which
they appear in the class definition: The first member is initialized first,
then the next, and so on. The order in which initializers appear in the
constructor initializer list does not change the order of initialization.
class X { int i; int j; public: X(int val): j(val), i(j) {} // actually i is
initialized first.  }

Default arguments and constructors: Sale_data(std::string s = ""): bookNo(s) {}
A constructor that supplies default arguments for "all" parameters also defines
the defualt constructor.

class member initialization if not explicitly initialized:
- for objects: their default constructor is called. e.g., std::string
- for primitive types(pointers, int), they are not initialized.
- for references: it is illegal not to initialized them.


## The IO Library Header files:
- iostream: istream, wistream reads from a stream; ostream, wostream writes to
  a stream;
- fstream: ifstream, wifstream; and ofstream, wofstream
- sstream: istringstream, wistringstream; ostringstream, ostringstream; and
  stringstream, wstringstream reads and writes a string.  The IO libraries also
define a set of types and objects that manipulate wchar_t data, e.g, wcin,
wcout, and wcerr. wchar_t is at least 16-bit arithmetic type.

The type ifstream and istringstream inherit from istream. >>, or << operations
can work for all thest types.

No copy or Assign for IO Objects Because we can't copy the IO types, we cannot
have a parameter or return type that is one of the stream types. Function that
do IO typically pass and return the stream through references. Reading or
writing an IO obeject changes its state, so the reference must not be const. 

Condition States strm::iostate. strm is one of the IO types listed. isostate is
a maxchine-dependent integral "type" that represents the condition state of a
stream.  strm::badbit => a stream is corrupted.  strm::failbit => an IO
operation failed strm::eofbit => hit end-of-file.  strm::goodbit => not in an
error state. The value is guranteed to be zero.  s.eof() => true if eofbit is
set.  s.fail() => true if failbit or badbit is set.  s.bad() => true if badbit
is set s.good => true if the stream s is in a valid state.  s.clear() => rest
all condition values in the stream s to be valid state. return void.
s.clear(flags) => RESET the condition of s to flags.  s.setstate(flags) => ADD
specified conditions to s. Error flag will not be turned off. It is like a
bitwise OR operation.  s.rdstate() => Return current condition of s as a
strm::iostate value. 

Once an error has occurred, subsequent IO operations on that stream will fail.
We can only read from or write to a stream only when it is in a non-error
state.  The easiest way to determine the state of a stream object is to use
that object as a condition: while (cin >> word). The while condition checks the
state of the stream returned from the >> expression.

*Interrogate the State of a stream* The badbit indicates a system-level
failure, such as an unrecoverale read or write error. The failbit is set after
a recoverable error, such as reading a character when numerci data was
expected. Reaching end-of-file sets both eofbit and failbit. In addition, fail
also return true if bad is set. If any of badbit, failbit, or eofbit are set,
then a condition that evaluates that stream will fail.  *Managing the condition
state* cin.clear(cin.rdstate() & ~cin.failbit & ~cin.badbit) to turn off
failbit and badbit but leaves eofbit untouched.

*Managing the output buffer* Each output steam manages a buffer, which it uses
to hold the data that the program reads and writes. Using a buffer allows the
operating system to combine several output operations from our program into a
single system-level write. Cuz writing to a device can be time-consuming.
Several conditions to cause the buffer to be flushed:
1. The progam complete normally. All output buffers are flushed as part of
return from main.
2. At some indeterminate time, the buffer can become full.
3. We can flush the buffer explicitly using a manipulator endl.
4. We can use 'unitbuf' manipulator to set the stream's internal state to empty
the buffer after each output operation. By default, unitbuf is set for cerr, so
that write to cerr are flushed immediately.
5. An output steam might be tied to another stream. Teh buffer of the tied
stream is flushed whenever the tied stream is read or written. By default, cin
and ceer are both tied to cout. Hence, reading from cin or writing to cerr
flushes the buffer in cout.

cout << "hi!" << endl; // write hi and a new line, then flushes the buffer cout
<< "hi!" << flush; //write hi, then flush; adds no data cout << "hi!" << ends;
// writes hi and a null, then flush the buffer

count << unitbuf; // flush immediately count << nounitbuf; // returns to normal
buffering

Caution: Buffers are not flushed if the program crashes.

Tying input and output streams together. The library ties cout to cin, so the
statement cin >> ival; causes the buffer assocaited with cout to be flushed.
Interactive systems usually shold tie their input stream to their output
stream.  cin.tie(); // return currently tied stream cin.tie(&cout); // tie to a
new stream cin.tie(nullptr) // untie cin

**File input and output** The fstream header defines three types to support
file IO:
- ifstream to read from a given file
- ofstream to write to a given file
- fstream to read and write a given file.

File stream types provides the same operations as those we have previously used
on the objects cin and cout.  In addition to the behavior that they inherit
from the iostream types, specific operations:
- fstream fstrm; // creates an unbound file stream.
- fstream fstrm(s); // creates an fstream and opens the file named s. s can
  have type string or can be a pointer to a C-style character string. The
default mode is depend on the type of fstream.
- fstream fstrm(s, mode);
- fstrm.open(s); // return void
- fstrm.open(s, mode); // return void
- fstrm.close();
- fstrm.is_open();

Using file stream objects When we want to read or write a file, we can use an
fstream in place of an iostream&.  Once a file stream has been opened, it
remains associated with the specified file. Calling open on a file stream that
is already open will fail and set failbit. To assoicate a file stream with a
different file, we must first close the existing file.  Automatic construction
and Destruction Because input is local to the while, it is created and
destroyed on each iteration. When an fstream object goes out of scope, the file
it is bound to is automatically closed. When an fstream object is destroyed,
close is called automatically.

File Modes:
- in; //input
- out; //output
- app; //append before every write
- ate; //seek to the end immediately after the open
- trunc; // truncate the file
- binary; // Do IO operation in binary mode

ofstream out("file1") // out and trunc are implicit ofstream out("file1",
ofstream::out)  // trunc is implicit ofstream app("file", ofstream::app); //
out is implicit

The only way to preserve the existing data in a file opened by an ofstream is
to specify app or in mode explicitly.  The file mode of a given stream may
change each time a file is opened.

* String streams * The sstream headers deins three types to support in-memory
  IO.  stringstream specific operations: sstream strm; // unbound stringstream
sstream strm(s); // strm is an sstream that holds a copy of the strings.
strm.str(); // Returns a copy of the string that strm holds.  strm.str(s); //
Copies the strings into strm. Return void.

An ostringstream is useful when we need to build bup our output a little at a
time but do not wan to print the output until later.

### Sequential Containers
Sequential containers: the order of elements in a sequential container
corresponds to the positions in which the elements are added to the container.
The order does not depend on the values of the elements.
- vector: flexible-size array. Supports random access. Inserting or deleting
  elements other than at the back may be slow.
- deque: Double-ended queue. Fast random access. Fast insert/delete at front or
  back.
- list: Doubly linked list. Support only bidirectional sequential access. Fast
  insert/delete at any pointer in the list.
- forward_list: Singly linked list. Supports only sequential access in one
  direction. Fast insert/delete at any point in the list.
- array: fixed-sized array. Supports fast random access. Cannot add or remove
  elements.
- string: A specialized container that contains characters. Fast random access.
  Fast insert/delete at the back.
Only array is a fixed-size container.
String and vector hold their elements in continguous memory. So random access
is efficient, but adding or removing elements in the middle takes time.
Moreover, adding an element can sometimes requires that additional storage be
allocated, cuz every element must be moved into the new storage.
The list and forward_list are designed to make it fast to add or remove an
elements anywhere in the container. No fast random access. The memory overhead
is substantial.
deque supports fast random access. As with string and vector, adding or
removing elements in the middle of a deque is a (potential) expensive
operation. However, adding or removing elements at either end of deque is a
fast operation, comparable to adding a element to a list or forward_list.
An array is a safer, easier-to-use altenative to built-int arrays, which has a
fixed-size.
A forward_list doesn't have the size operation because storing or computing its
size would entail overhead compared to a handwritten list.
For other containers, size is guaranteed to be a fast, constant-time operation.

Modern C++ programmers should use the library containers rather than more
primitive structure like arrays.

Ordinarily, it is best to use vector unless there is a good reason to prefer
another container.

Rule of thumbs:
1. Unless you have a reason to use another container, use a vector;
2. If your program has lots of small elements and space overhead matters, don't
   use list or forward_list.
3. If it requires random access to elements, use a vector or a deque.
4. If it needs to insert or delete elements in the middle of the container, use
   a list or forward_list.
5. If the program needs to insert or delete elements at the front and the back,
   but not in the middle, use a deque.
6. if u need to insert elements in the middle, and then random access: first,
   decide whether you actually need to add elements in the middle of a
container. It is easier to append to a vector and then call the library sort
function. If you must insert in the middle, use a list for the input phrase,
then copy the list into a vector.
In general, the predominant operation of the application will determine the
choice of container type.
If you're not sure which container to use, write your code so that it uses only
operations common to both vectors and lists: use iterators,not subsripts, and
avoid random access to elements.

##Container library overview
Common operations:
1. Type aliases:
    - iterator: type of the iterator for this container type
    - const_iterator: iterator type that can read but not change its elements
    - size_type: Unsigned integral type big enough to hold the size of the
      largetst possible container of this container type
    - difference_type: signed integral type big enough to hold the distance
      between two iterators
    - value_type: element type
    - reference: element's lvalue type; value_type&
    - const_reference: const value_type
2. Construction:
    - C c; Default constuctor, empty container
    - C c1(c2); construct c1 as a copy of c2;
    - C c(b,e); Copy elements from the range denoted by iterators b and e;(not
      valid by array);
    - C c{a,b,c...}; List initalize c
3. Assignment and swap
    - c1 = c2; Replace elements in c1 with those in c2;
    - c1 = {a,b,c...} replace elements in c1 with those in the list(not valid
      for array);
    - a.swap(b); swap elements in a with those in b
    - swap(a,b); Equivalent to a.swap(b)
4. Size
    - c.size(); not valid for forward_list
    - c.max_size(); Maximum number of elements c can hold
    - c.empty(); false if c has any elements
5. Add/Remove elements(not valid for array)
    - c.insert(args); Copy elements into c
    - c.emplace(inits); use inits to construct an element in c
    - c.erase(args);
    - c.clear(); Remove all elements from c; return void
6. Equality and Relational operators:
    - ==, != ; valid for all container types
    - <, <=, >, >=; not valid for unordered associative containers
7. Obtain Itertors
    - c.begin, c.end(); Return iterator to the first, one past the last element
      in c
    - c.cbegin(), c.cend(); Return const_iterator
8. Additional Member of reversible containers(not valid for forward_list)
    - reverse_iterator
    - const_reverse_iterator
    - c.rbegin(), c.rend()
    - c.crbegin(), c.crend()
###Defining and initializing Containers
C c; Default constructor. If C is array, then all the elements in c are
default-initialized; otherwise c is empty.
C c1(c2); or C c1 = c2; c1 is a copy of c2. For array, they must have the same
size.
C c{a,b,c..l} c is a copy of the elements in the initializer list.
C c(b,e) c is a copy of elements in the range denoted by iterators b and e.(not
valid for array).
Constructors that take a size are valid for sequential containers(not including
array) only:
1. C seq(n); seq has n value-initialized elements;
2. C seq(n,t); seq has n elements with value t;
Each container is defined in a header file with the same name as the type.

### Iterators
Iterators have a common interface.
- *iter: return a reference to the element denoted by the iterator iter
- iter->mem: Dereference iter and fetches the member named mem from the
  underlying element. Equivalent to (*iter).mem
- ++iter: Increments iter to refer to the next element in the container
- --iter: Decrements iter to refer to the previous element in the container
- iter1 == iter2: Whether they denote the same element
- iter1 != iter2: different element
forward_list doesn't support decrement(--) operator. The iterator arithmetic
operations apply only to iterators for string, vector, deque, and array.
An iterator range [ begin, end )
Container type members.
e.g., vector<int>::size_type index;
list<string>:: reference string_reference;
deque<string>:: value_type string_member;
deque<string>::const_reference string_member; // to read string member
deque<string>:: reference foo; // to write

auto begin = a.begin();
auto end = a.cend();
When write access is not needed, use cbegin and cend.

## Defining and initializing a container
1. Initialize a container as a copy of another container
To create a container as a copy of another container, the container and element
types must match. When we pass iterators, there is no requirement that the
deque<string> foolist(a.begin(), it);
vector<string> foolist(fishList);
container types be identical. Even the element type can be converted.
2. List initialization
list<string> authors = {"a", "b", "c"};
3. Sequential container size-related constructors
We can initialize the sequential containers(other than array) from a size and
an(optional) element initializer.
vector<int> ivec(10,-1);
list<string> svect(10, "hi!");
forward_list<int> ivec(10);
We can use the constructor that takes a size argument if the element type is a
built-in type or a class type that has a default constructor. If the element
type does not have a default constructor, then we must specify an explicit
element initializer along with the size.

Library arrays have fixed size
Just as the size of a built-int array is part of its type, the size of a
library array is part of its type.
array<int, 11> // array htat holds 11 ints.
array<int,10>::size_type i;
The size is part of the array's type.
array<int,10> ia = {42}; // ia[0] is 42, remaining elements are 0.
We cannot copy or assign objects of built-int array types, but there is no such
restriction on array:
int digs[10] = {0,1,2,3,4,5,6,7,8.9};
int cpy[10] = digs; //error
array<int,10> digits = {0,1,2,3,4,5,6,7,8,9};
array<int, 10> copy = digits;

Assignment and swap
c1 = c2; //replace the contents of c1 with a copy of the elements in c2
c1 = {1,2,3}; // c1 has size 3.
c1.swap(c2); swap(c1, c2) // swap is usually faster than copying elements from
c2 to c1;
* Assignment operations not valid for associative containers or array
seq.assign(b,e) //replace elements in the seq with those in the iterator range
seq.assign(i1) // replaces the elements in seq with those in the initializer
list i1.
seq.assign(n,t); // replace the elements in the seq with n elements with value
t.
Because the size of the right-hand operation may differ from the size of the
left-hand operand, the array type doesnot support assign and it does not allow
assignment from a braced list of values.
The sequential containers(except array) also define a member named assign that
lets us assign from a different but compatible type, or assign from a
subsequence of a container.

Using swap:
swap(v1, 2); // the elements themselves are not swapped; internal data
structures are swapped. Excepting array, swap is guaranteed to run in constant
time.
**Unlike how swap behaves for other containers, swapping two arrays does
exchange the elements.
The containers offer both a member and nonmember version of swap. As a matter
of habit, it is best to use the nonmember version of swap.

Container size operations:
size(), empty(), max_size(); // forward_list without a size()

Relational operators:
Every container type supports the equality operations(== and !=); all the
containers except the unordered associative containers also support the
relational operators(>, >=, <=, <). The right- and left-hand operands must be
of the same kind.
**We can use a relational operator to compare two containers only if the
appropriate comparision operator is defined for the element type.

##### Sequentiall container operations
Excepting array, all of the library containers provide flexible memory
management.
c.push_back(t); c.emplace_back(args); //creates an element with value t or contructed from args at
the end of c. Return void.
c.push_front(t); c.emplace_front(args);
c.insert(p,t); c.emplace(p, args) // creates an element with value t or constructed from args
before the element denoted by iterator p. Return an interator referrring to the
element that was added.
c.insert(p,n,t); // insert n elements with value t before the element denoted
by iterator p. Return an iterator to the first element inserted.
c.insert(p,b,e); // iterator b and e.
c.insert(p,i1); // i1 is a braced list of element values.
Adding elements anywhere but at the end of a vector or string, or anywhere but
the beginning or end of a deque, requires elements to be moved. Moreover,
adding elements to a vector or a string may cuase the entire object to be
reallocated.

1. push_back (except forward_list and array)
Aside from array and forward_list, every sequential container(including the
string type) supports push_back.
Key Concepts: Container elements are copies. Whether initializing or copying,
the original element and the copied element has no relation any more.
2. push_front(list, forward_list, deque)
3. Adding elements at a specified point in the container
supported for vector, deque, list and string. It takes a iterator as the first
parameters. 
It is legal to insert anywhere in a vector, deque, or string. However, doing so
can be an expensive operation.

2. push_front(list, forward_list, deque)
3. Adding elements at a specified point in the container
supported for vector, deque, list and string. It takes a iterator as the first
parameters. 
It is legal to insert anywhere in a vector, deque, or string. However, doing so
can be an expensive operation.
svect(iterator, foo);
svect(iterator, number, foo);
svect(iterator, iter_a, iter_b);
svect(iterator, {});
When we pass a pair of iterators, those iterators may not refer to the same
container as the one to which we are adding elements.
We can use the iterator returned from insert

4. Using emplace operations
emplace, emplace_back, emplace_front construct rather than copy elements.
c.emplace_back("a", 1, "acf"); // the object is created directly in space
managed by the container.
c.push_back(Foo("a", 1, "acf")); // creates a local temporary object that is
pushed onto the container.
The arguments to an emplace function vary depending on the element type.

##### accessing elements
Access elements in a sequential container
at and subscript valid onlh for string, vector, deque, and array; back not
valid for forward_list;
The access operations are undefined if the container has no elements;
c.back(); returns a reference to the last element in c;
c.front(); // if empty, undefined.
c[n]; // if out of range, undefined.
c.at(n); // If the index is out of range, throws an out_of_range exception.
The access members return references. If not constant, we can use this to
modify the value of the fetched element.
auto &v = c.back(); // get a reference to the last item; we must define it as a
reference type.
auto v = c.back(); // get a copy of c.back();
Using subscription to access out-of-range vaue is a serous programming error
that the compiler will not detect. Use at instead.
c.pop_back(); c.pop_front(); // undefined if c is empty. Return "void"
c.erase(p); //removes the element denoted by th iterator and returns an
iterator to the element after the one deleted or the off-the-end iterator.
Undefined if p is off-the-end iterator.
c.erase(b,e); // iterator range
c.clear(); // removes all.
The memebers that remove elements do not check their arguments. The programmer
must ensure that elements exist before removing them.
If you need the value you are about to pop, you must store the value before
doing the pop.
insert and erase all use iterator to denote position.

**Specialized forward_list operations
lst.before_begin(); lst.cbefore_begin()// iterator denoting the nonexistent element just before the
beginning of the list. The is iterator may not be dereferenced.
lst.insert_after(p,t);
lst.insert_after(p,n,t);
lst.insert_after(p,b,e);
lst.insert_after(p,i1); // Undefined if p is the off-the-end iterator.
lst.emplace_after(p, args);
lst.erase_after(p); // return an iterator to the element after the one deleted.
lst.erase_after(b,e); // after b, up to but not including e.
When we add or remove elements in a forward_list, we have to keep track of two
iterators - one to the element we're checking and one to the element's
predecessor. _-

Resizing a container:
ilist.resize(a); // resize to size a
ilist.resize(a,b); //resize to size b with default value b for new element.

##### Container operations may invalidate iterators
An invalid pointer, reference, or iterator is one that no longer denotes an
element. Using it is like using an uninitialized pointer.
If adding leads to reallocation, all are invalid.
When you use a iterator(or a reference or pointer to a container element), it
is a good idea to minimize the part of the program during which an iterator
must stay valid.

**Avoid storing the iterator returned from end.
Don't cache the iterator returned from end() in loops that insert or delete
elements in a deque, string, or vector. We must recompute it after each
iteration.

Lvalues and Rvalues
An expression is composed of one or more operands and yields a result when it
is evaluated. The simplest form of an expression is a single literal or
variable.
Every expression in C++ is either an rvalue or an lvalue. In C++, an lvalue
expression yields an object or a function. Roughly speaking, when we use an
object as an rvalue, we use the object's value(its contents). When we uses an
object as an lvalue, we use its identity(its location in memory).
Operators differe as to whether they requires lvalue or rvalue operands and as
to whether they return lvalues or rvalue. The important point is that we can
use an lvalue when an rvalue is required, but we cannot use an rvalue when an
lvalue is required.
Assignment requries a lvalue as its left-hand operand and yields its left-hand
operand as an lvalue;
The address-of operator requires an lvalue operand and returns a pointer as an
rvalue;
The dereference and subscript operators and the iterator dereference all yield
lvalues;
The built-int and iterator increment and decrement operators require lvalue
operands and the prefix version also yield lvalues.
dectltype(*p); // because derefrerence yields an lvalue, it is int&.
dectltype(&p); // because &p yields an rvalue, it is int**;

Pointers are iterators; but not vice versa. -_

#### How a vector grows
vector elements are stored continguoulsy. For string and vector, library
implementation use allocation strategries that reduce the number of times the
container is reallocated, which is allocating more than what is immediately
needed. This makes vector grow more efficiently than a list or a deque.

deque is implemented as a vector of fixed chunks, and that's why it has fast randam
access.
member to manage capacity:
1. shrink_to_fit() // request to reduce capacity to equal size(); But the
   implementation is free to ignore this request.
2. capacity() // number of elements c can have before allocation is necessary.
3. reserver(n) // allocate space for at least n elements, but it will never
   reduce the capacity.
resize() only changes the number of elements in the container, not its
capacity.

The size and capacity of an empty vector is zero.
Calling shrink_to_fit() is only a request; there is no guarantee that the
library will return the memory._

### String
Initialization:
1. string s1; Default initialization; s1 is the empty string;
2. strings2(s1) or string s2 = s1; s is a copy of s1;
3. string s("abc") or string s = "abc"; a copy of the string literal. Not
   including null termiator.
4. string s4(n, 'c') // initialize s with n copies of the 'c'
5. string s(cp, n); // a copy of the first n characters in the cstring array.
6. string s(s2, pos2); // a copy of the string s2 starting from pos2.
7. string s(s2, pos2, len2); a copy of the string s2 string from pos2 to at
   most its end.

When we create a string from a const char*, if we do not pass a count and there
is no null, or if the given count is greater than the size of the array, the
operation is undefined.
When we copy from a string, if the optional starting position is greater than
the size of, the constructor throws an out_of_range exception.

*substr operation
s2.substr(1);
s2.substr(1,7); // return a string containing n characters from s starting at
pos. pos default to 0. n defaults to a value that can copy all the characters
in s starting from pos.

In addition to taking iterators, string provides versions that take an index.
a lot of overloaded version of insert and assign.
append, replace();
The range can be an index and a length, or a pair of iterators.
The characters can be taken from another string, from a character pointer, from
a brace-enclosed list of characters, or as a charactr and a count.

* Seach operations
returns a string::size_type value. if no match, returns a static member named
string::npos. 
The library defines: 
const string::size_type npos = -1; // means npos is equal to the largest
possbile size any string could have.
s.find(args);
s.rfind;
s.find_first_of(args); //find the first occurrence of any character from args in s.
s.find_last_of(args); //  last
s.find_first_not_of(args);
s.find_last_not_of(args);j
overloaded compare functions similar to strcmp;

*Numeric conversions
The character representation of a number differ from it numeric value.
to_string(val); // val can be any arithmetic type.
stoi(s,p,b);
stol, stoul, stoll, stoull
stof, stod, stold.
If the string can't be converted to a number, these functions throw an
invalid_argument exception. If the converstion generates a value that can't be
represented, they throw out_of_range.

### Container Adaptors
three sequential container adaptors: stack, queue, and priority_queue.
Common operations and types:
size_type, value_type, container_type, A a; A a(c); relational operators;
empty(), size(), swap(a,b), a.swap(b).
By default, bothe stack and queue are implemented in terms of deque, and a
priority_queue is on a vector.
stack<string, vector<string>> str_stk; // implemented on top of vector.
stack<string, vector<string>> str_stk(sve);
They cannot be built on array, a fixed-size container, or forward_list, which
cannot access the last element. The queue adptor cant not be on a vector. A
priority_queue can be on vector or deque, but not on a list.
stack operations:
pop(), push(item), emplace(args), top()
We can use only the adaptor operations and cannot use the operations of the
underlying container type.
queue operations:
pop(), front(), back(), push, emplace
priority_queue:
pop(), top(), push, emplace.
By default, the library use the operator < on the element type to determine
relative priorities.

## Generic algorithms
Rather than adding lots of functionality to each container as Java, the library
provides a set of algorithms, most of which are independent of any particular
container type. These algorithm are generic: They operate on different types of
containers and on elements of various type.
Most of the algorithms are defined in the algorithm header. The library also
defines a set of generic numeric alogirthms that are defined in the numeric
header.
In general, the algorithms do not work directly on a container. Instead, they
operate by traversing a range of elements bounded by two iterators.
find(vec.cbegin(), vec.cend(), val);
For built-int array, we use the library begin and end functions. We can also
look in a subrange of the sequence by passing iterators(or pointers).

Iterators make the algorithms container independent: it either returns an
iterator denoting the position or off-the-end iterator.
But algorithms do depend on element-type operations, e.g., == for comparision,
or < for sorting.
** Algorithms never execute containr operations. They might change the values
of the elements, or move them around.
The algorithms always use the first parameters as "input range".

1. read-only algorithms: find, count, accumulate(begin, end, initial)
in accumulate(defined in numeric), the elements in the sequence must match or be convertible to the
type of the third argument, like double to int. The type of the third argument
to accumulate determines which addition operator is used and is the type that
accumulate returns.
accumulate(v.cbegin(), v.cend(), string(""));
accumulate(v.cbegin(), v.cend(), "");  // this is error. As the third argument
is const char*, and there is no + operator for type const char*.

equal(a.cbegin(), a.cend(), b,cbegin());
Differently from "==", we can call equal to compare elements in containers of
different types and moreover, the element types also need not be the same as
long as we can use == to compare the element types, e.g, vector<string>, and
list<const char*>

* Algorithms that write container elements
fill(vec.begin(), vec.end(), 0);
Algorithms that write to a destination iterator assume that the destination is
large enough to hold the number of elements being written.
fill_n;
copy(a1.i1, a1,i2, a2);
replace(ilist.begin(), ilist.end, a, b);
back_insert returns an insert iterator. When we assign through an insert
iterator, a new element equal to the right-hand value is added to the
container.
sort, unique,
The library algorithms operate on iterators, not containers. Therefore, an
algorithm cannot direclty add or remove elements.

* Passing a function to an algorithms
A predicate is an expression that can be called and that returns a value that
can be used as a condition. The predicates used by library algorithms are
either unary predicates or binary predicates.
sort(iterator1, iterator2, sort_function);
stable_sort(i1, i2, s);
for_each(i1, i2, lambda);

There are some similarity between Python and C++.

Callable: classes that overloads the function-call operator, and lambda
expressions.
lambda: [capture list] (paremeter list) -> return type {function body}
lambdas with function bodies that contain anything other than a single return
statement that do not specify a return type return void.
A lambda can only use the variables local to that function captured in the
capture list.
The capture list is used for local nonstatic var only; lambdas can use local
statics and variables declared outside the function directly.
When we define a lambda, the compiler generates a new (unnamed) class type that
corresponds to that lambda.
lambda capture list:
[]: empty capture list
[names]: comma-seperated. By default captured by value; if &, by reference
[&]: implicity by reference.
[=]: by value;
[&, identifier_list]: implicit by reference; explicit by value;
[=, reference_list]:  implicity by value; explicit by reference.
Advice: keep your lambda captures simple.
By default, a lambda may not change the value of a var that it copies by value.
If we want, we must follow the parameter list with the keyword `mutable`.
When we need to define a return type for a lambda, we must use a trailing
return type.
transform(i1, i2, b1, function);

find_if takes only a unary predicate, so the callable passed to find_if must
take a single argurment, or use a lambda.
Or we can use bind, defind in `functional` header.
auto newCallable = bind(callable, arg_list);
bind(call, _1, _2, foo);
using std::placeholders::_1; // use only one from that namespace
using namespace namespace_name; // use all names from that namespace.
bind(print, ref(os), _1, ' ') // use ref() to solve uncopyable argument.

#### Iterators
1. insert iterators
back_inserter creates an iterator that uses push_back.
front_inserter
inserter
2. iostream Iterators
3. reverse iterators
rcomma.base()
4. move iterators

* Container-specific algorithms
list and forward_list define several algorithms as memebers.
sort cannot be used with list and forward_list because these types offer
bidrectionaly and forward iterators, resectively.
The list member versions should be used in preference to the generic algorithms
for lists and forward_lists.
lst.splice, flst.splice

### Associative containers
associative container is accessed by key, while sequential is by position.
map and multimap in map header;
set and multiset ins set header;
unordered containers are in unordered_map and unordered_set headers.
like the sequential containers, the associative containers are templates.
we get a pair object.

Requirements on key type:
Strict weak ordering.
for sorted type, By default, the library use the < operator for the key type to compare the
keys.
multiset<Sales_data, decltype(compareIsbn)*> bookstore(compareIsbn);
pair is defined in the utility header.
make_pair(v1, v2) or {} to return a pair.
value_type of a map is a pair.
Iterators for sets are const.
Associative containers can be used with the algorithms that read elements.

Adding elements: insert
erase

subscripting a map:
c[k]; c.at(k);
Subscripting a map behaves quites differently from doing it on an array or
vector.
c.find, count, lower_bound, upper_bound, equal_range,

Use an unordered container if the key type is inherently unordered or if
performance testing reveals problems that hashing might solve.
Mananging the buckets:
c.bucket_count();c.max_bucket_count(); c.buckdet_size(n);c.bucket(k);
local_iterator; c.begin(n); c.load_factor(); c.max_load_factor(); c.rehash();
c.reverse(n);

We can direclty define unordered containers whose key is one of the built-in
types(including pointer types), or a string, or a smart pointer, because the
library supplies versions of the hash template for the built-in types..
Using SD_multiset = unordered_multiset<Sales_data, decltype(hasher)*,
decltype(eqOp)*>;
SD_multiset bookstore<42, hasher, eqop);
Unordered containers use the key type's == and an object of type hash<key_type>
to organized their elements.


### Dynamic memory and smart pointers
in memory header, shared_ptr, unique_ptr, and weak_ptr.
Like vectors, smart pointers are templates.
shared_ptr<string> p1;
A default smart pointer holds a null pointers.
shared_ptr<T> sp; // Null smart pointer that can point to objects of type T.
 unique_ptr<T>; p.get() // return the pointer in p.
operations common to shared_ptr and unique_ptr;
shared_ptr<T>, unique_ptr<T>, p, *p, p->mem, p.get(), swap(p,q), p.swap(q);

Specific to shared_ptr:
make_shared<T>(args); // use args to initialize the object.
shared_ptr<T> p(q); // p is a copy of the shared_ptr q; increments the count in
q.
p = q; // decrement p's reference count and increments q's count; delete p's
existing memory if p's count goes to 0.
p.unique(); // return true if p.use_count() is 1
p.use_count();

safest way to allocate and use dynamic memory:
shared_ptr<int> p2 = make_shared<int>(42);
or auto p = make_shared<vecotr<string>>();
make_shared uses its arguments to construct an object of the given type.

It is up to the implementation whether to use a counter or another data
structure to keep track of how many pointers share state. the key point is that
the class keeps track of how many shared_ptrs point to the same object and
automatically frees that object when appropriate.
Shared_ptr automatically destorys its objects through a destructor. If the ref
count goes to zero, the shared_ptr destroys the objects to which the shared_ptr
points and frees the memory used by that object.
If you put shared_ptrs in a container, and you subsequently need to use some,
but not all, of the elements, remember to erase the elements you no longer

One common reason to use dynamic memory is to allow multiple objects to share
the same state.
initializer_list parameter defined in initializer_list header, and it takes an
unknown number of arguments of a single type.
The elements in an initializer_list are always const values.
When calling, function({a,b,c});
Or Ellipsis parameters: void foo(par...), or void foo(par, ...); // C varargs

StrBlob::StrBlob(initializer_list<string> i1): data(make_shared<vector<string>(i1)) {}

By default, dynamically allocated objects are default initialized, which means
that objects of built-in or compound type have undefined value; objects of
class type are initialized by their default constructor.

For the same reason as we usually initialize variables, it is also a good idea
to initialize dynamically allocated objects.

auto p1 = new auto(obj);
