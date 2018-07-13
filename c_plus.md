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
unique_ptr<T>; 
p.get() // return the pointer in p.
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

*7.6 static class members*
A static member is associated with the class. it can be public or private, and
can be const, reference, array, class type and so forth. static member
functions may not be declared as const, and we my not refer to `this` in the
body of a static member, or implicit use `this` by calling a nonstatic member.

We can *define* a static member function inside or outside of the class body.
When we define a static member outside the class, we do not repeat the static
keyword. The `static` keyword is used only inside the class body.

In general, we may not initialize a static member inside the class. Instead, we
must define and initialize each static data member outside the class body.

Tips: the best way to ensure the object is defined exactly once is to put the
definition of static data members in the same file that contains the
definitions of the class noninline member functions.

In-class initialization of static data members
Ordinarily, static members may not be initialized in the class body. We can
provide in-class initializer for static members that have const integral type
and must do so for static members that are constexpr of literal types.

Best practice: Even if a const static data member is initialized in the class
body, that member ordinarily should be defined outside the class definition. If
an initializer is provided inside the class, the member's definition must not
speficy an initial value.
static members can have imcomplete types.

class Screen; //declaration of the Screen class: an incomplete type
We can use an incomplete type in only limited ways:
- define pointers or references to such types
- declare(but not define) functions that use an incomplete type as a parameter
  or return type.
A data member of a class can be of its own class, as a class is not defined
until its class body is complete, hence it cannot know the needed storage for
that data member.

*12.1.2 Managing memory directly*
Classes that do manage their own memory cannot rely on the default definitions
for the members that copy, assign, and destroy class objects.
int *pi = new int; //pi points to a dynamically allocated, unnmaed,
uninitialized int.
By default, for dynamically allocated objects, built-int or compund type have
unfined value; objects of class type are initialized by their default
constructor.
String *ps = new string; // empty string;
int *pi = new int; // uninitialized int.
string s; // empty string
string s2(s1) equal to s2 = s1;
int *pi = new int();; // initialized to 0.
We can also use list initialization:
vector<int> *pv = new vector<int>{0,1,2,3,4}
For built-ins, value initialization () and default initialization is different,
while it is the same for objects of class type.

auto p1 = new auto(obj); // use obj to deduce the type

const int *pi = new const int(1024); // must be initialized.
A const dynamic object of a class type that defines a default constructor may
be initialized implicitly, but other types must be explicitly initialized.

If there is memory exhaustion, a `bad_alloc` will be thrown.
But: int *p2 = new (nothrow) int; // if allocation fails, new return a null
pointer. This kind of new is called replacement new. Both bad_alloc and nothrow
are defined in the new header.

Free dynamic memory:
deleting a pointer to memory that was not allocated by new, or deleting the
same pointer more than once, is undefined.
int *pi2 = nullptr;
delete pi2; // ok to delete a null pointer.
Compilers cannot tell whether a pointer points to a statically or dynamically
allocated object, or whehter memory addressed by a pointer has already been
freed.

Foo& get_copy_reference() { return *this; } // used for chaining.

Dynamic memory managed through built-in pointers exists until it is explicitly
freed.

Caution: managing dynamic memory is error-prone:
1. forgetting to delete memory, resulting in memory leak. Testing for memory
   leak is difficult because they usually cannot be detected until the
application is run for a long enough time to actually exhaust memory.
2. using a deleted pointer. This error can sometimes be detected by making the
   pionter null after the delete.
3. deleting the same memory twice, and then the free store may be corrupted.

No dangling pointer.

* Smart shared_ptr *
shared_ptr<double> p1;
shared_ptr<int> p2(new int(42)); // no implicit converting built-in pointers
cuz its constructor takes explicit pointer.
sharea_ptr<int> p2 = new int(11); // **WRONG*****
shared_ptr<T> p(q); // p manages the object to which the built-in pointer
points;
shared_ptr<T> p(u); p assumes ownership from the unique_ptr u; make u null;
shared_ptr<T> p(u);
shared_ptr<T> p(q,d); // use the callable object d in place of delete.
p.reset(); // if p is the only shared_ptr, reset frees p's existing object.
p.reset(n_ptr); // use reset to assign a new pointer to p. We cannot use '='
shared_ptr<int> clone (int p) {
    return shared_ptr<int>(new int(p));
}

When we bind a shared_ptr to a plain pointer, we give responsibility for that
memeory to that shared_ptr. And we should not no longer use a built-int pointer
to access the memory to which the shared_ptr now points.

Don't use `get` to initialize or assign another smart pointer. get() is
intended for cases when we need to pass a built-int pionter to code that can't
use a smart pointer. The code using the pointer returned from get must not
delete that pointer.

if (!p.unique()) {
    p.reset(new string(*p)); // allocate a new copy
}

*Smart pointers and exceptions*

C++ exception will propragate along the function chain, like in python. If no
one is caught, execution is transferred to a library function named
`terminate`.

C++: Writing exception safe code is Hard.  `exception` header: the most general
kind `stdexcept`: several general-purpose exception classes `new`: the
bad_alloc exception type `type_info`: bad_case type Provide initializer to
provide additional information about the error that occurred.

When a function is terminated, whether through normal processing or due to an
exception, all the local objects are destroyed. So if we use a shared_ptr to
manage memory, that will be freed automatically, while not if using raw memory.

By default, when a shared_ptr is destroyed, it execute delete on the pointer it
holds. We can define a function to use in place of delete.
void end_connection(*p){ disconnect(*p);}
shared_ptr<connection> p (&c, end_connection);

Caution: smart pointers pitfalls
1. Don't use the same built-int pointer value to initialize(or reset) more than
   one smart pointer.
2. Don't delete the pointer returned from get().
3. Don't use get() to initialize or reset another smart pointer.
4. If you use a pointer returned by get(), that pointer will become invalid
   when the last responding smart pointer goes away.
5. If you use a smart pointer to manager a resource other than memory allocated
   by new, remember to passer a deleter function.

** unique_ptr **
unique_ptr<T> u1; // null unique_ptrs that can point to objects of type T.
unique_ptr<T, D> u2; // use a callable object of type D to free its pointer.
unique_ptr<T, D> u(d); // d is a object of type D.
u = nullptr; // deletes the object; makes u null
u.release(); // relinquish control of the pointer u had held; returns the
pointer u had held and makes u null;
u.reset(); // delete
u.reset(q); // point to a built-in pionter q.
u.reset(nullptr); // makes u null;
Because unique_ptr owns the object, it does not support ordinary coy or
assignment.
unique_ptr<string> p2(p1.release());
p2.reset(p3.release()); // transfer ownership from p3 to p2; delete the memory
to which p2 had pointed.

Calling release() just breaks the connection between a unique_ptr and the
object it is managing.
p2.release(); // p2 won't free the memory.
auto p = p2.release(); // we need to delete(p).

Exception: we can copy or assign a unique_ptr that is about to be destroyed.
unique_ptr<int> clone(int p) {
    return unique_ptr<int>(new int(p));
} // the compiler does a special copy
or
unique_ptr<int> clone(int p) {
    unique_ptr<int> ret(new int(p));
    return ret;
} // the compiler does a special copy

* Passing a Deleter to unique_ptr *
Similar to overriding the comparison operation of an associative container, we
must supply the deleter type inside the angle brackets along with the type to
which the unique_ptr can point, which affect the unique_ptr type.
Different from shared_ptr, we must
unique_ptr<connection, decltype(end_connection) *> p(&c, end_connection); // *
indicates we're using a pointer to that type.

** 12.1.6 weak_ptr **
A weak_ptr does not control the lifetime of the object.
weak_ptr<T> w; // null weak_ptr
weak_ptr<T> w(sp); // points to the same object as the shared_ptr sp;
w = p; // p can be a shared_ptr or weak_ptr;
w.reset(); // makes w null;
w.use_count(); // the number of shared_ptrs that share ownership with w.
w.expired(); // if w.use_count() == 0;
w.lock(); // If expired() returns true, returns a null shared_ptr; otherwise
returns a shared_ptr to the object to which w points.
auto p = make_shared<int>(42);
weak_ptr<int> wp(p);
Because the object might no longer exist, we cannnot use a weak_ptr to access
its object directly. We must call lock.
if (shared_ptr<int> np = wp.lock()) {} // access the object only if np is not
null.
By using a weak_ptr, we don't affect the lifetime of object. However, we can
prevent the user from attempting to access a object that no longer exists.

### 12.2 Dynamic Array ###
Most applications should use a library container rather than dynamically
allocated arrays. Using a container is easier, less likely to contain
memory-management bugs, and is likely to give better performance.

typedef int arrT[42]; // names the type array of 42 ints.
int *p = new arrT; // the compiler will tranform it to new int[42];

Essentially, allocating an array yields a pointer to the element type.
int ia[] = {1,2,3]}; int *beg = begin(ia); // dimension is part of the array
type.
But dynamically allocated array is not an array type, so we cann't call begin
or end on it. For the same reason, we also cannot use a range for to process
the elements in a (so-called) dynamic array.

int *p = new int[10]; // uninitialized int;
int *p2 = new int[10](); // initialized to 0
int *p3 = new int[10](1,2,3,4); //error
int *p4 = new int[10]{1,2,3,4,5}; // we cannot use 'auto' to allocate an array.

It is legal to dynamically allocate an empty array, while not to statically do
it.

delete [] pa; // destroyed in reverse order.
delete pa; // undefined behavior

unique_ptr<int[]> up(new int[10)]); //
up.release(); // calls delete [];
up[0] = 3; // we cannot use . or arrow accessor, but we can use subscript.

shared_ptr<int> sp(new int[10], [](int *p) {delete[] p; }) // no directly
support for dynamical arrays in shared_ptr.

There is no subscript operator for shared_ptrs, and smart pointer types do not
support pointer arithmetic.

### 12.2.2 The allocator class ###
To decouple initialization and allocation, `allocator` provides raw,
unconstructed memory.
allocator<T> a;
a.allocate(n);
a.deallocate(p, n); // must destroy any object before calling deallocate.
a.construct(p, args);
a.destroy(p);
Once the elements have been destroyed, we can either reuse the memory to hold
ohter strings or return the memory to the system.

algorithms to copy and fill uninitialized memory:
uninitialized_copy(be, e, b2);
uninitialized_copy_n
uninitialized_fill/_n

#Part III Tools for Class authors#
Classes also control what happens when objects are copied, assigned, moved, and
destroyed. In this respect, C++ differs from other languages.

Sales_data obj; // declare an object initialized with the default constructor
Sales_data obj(); // oops! declare a function.

When a constructor is declared explicit, it can be used only with the direct
form of initialization. Moreover, there is no automatic conversion.
Sale_data obj(args);
Sale_data obj = obj2; // wrong, cannot use the copy form of initialization.
And the explicit keyword is meaningful only on constructors that can be called
with a single argument.
explicit constructors are only declared in class body.

## Copy Control ##
A class controls what happens when objects of that class type are copied,
moved, assigned, and destroyed, by defining: copy constructor, copy-assignment
operator, move constructor, move-assignment operator, and destructor. We call
them copy-control.
If a class does not define all of the copy-control members, the compiler
automatically defines the missing operations.

* The copy constructor *
A constructor is the copy constructor if its first parameter is a reference to
the class type and any additional parameters have default values;
class Foo {
    public:
        Foo(const Foo&);
} // as it is used implicitly in several circumstances, the copy constructors
usually should not be explicit.

* The Synthesized copy constructor *

We cannot copy or assign an arry to another.

A copy constructor is synthesized even if we define other constructors.
Mostly, the compiler copies each nonstatic member in turn from the given object
into the one being created. Member of class type are copied by the copy
constructor for that class; members of built-in type are copied directly. Array
type is copied by coping each element.

string dots(10, '.'); //direct initialization
string s(dots); // direct; copy constructor
string s2 = dots; // copy initialization
string lines = string(100, '9'); //copy

If a class have a move constructor, the copy initialization sometimes uses
themove constructor instead of the copy constructor.

Copy initialization happens when:
1. using a = to initialize
2. pass an object as an argument to a parameter of nonreference type
3. return an object from a function that has a nonreference return type
4. brace initialize the elements in an array or the members of an aggregate
   class.

The library containers copy initialize their elements when we initialize the
container, or we call insert or push member. By contrast, elements created by
an emplace member are direct initialized.

Tha fact that the copy constructor is used to initialize nonreference
parameters of class type explains whey the copy constructor's own parameter
must be a reference.

vector constructor that takes a single size is explicit.
So vector<int> v(10); //ok
vector<int> v2 = 10; // error
During copy initialization, the compiler is permitted to skip the copy/move
constructor and create the object directly.

* The copy-assignment operator *
The compiler will synthesizes a copy-assignment operator if the class does not
define its own.
Sale_data trans, accum;
trans = accum; // use copy-assignment operator.

Overloaded operators are functions that have the name operator followed by
symbol for the operator being defined, e.g., the assignment operator is
operator=.
Foo & operator=(const Foo&); // assignment operator
It usually return a reference to its left operand.
It assign each nonstatic member of the right-hand object to the corresponding
member fo the left-hand object using the copy-assignment operator for the type
of that member.

* The destructor *
~Foo(); // has no return value and taks no parameters.
There is always only one destructor for a given class.
In constructor, members are initialized first before the function body is
executed, and members are initialized in the same order as appear in the class.
In a destructor, the function body is executed first and then the members are
destroyed.
TThere is nothing to control how members are destroyed. Members of class type
are destroyed by running the member's own destructor.
Note: the implicit destruction of a member of built-in pointer type does not
delete the object to which that pointer points.
The built-in type does not have destructor.

When a destructor is called:
1. variable goes out of scope.
2. member of an object when the object is destroyed.
3. elements in a container when a container or an array are destroyed.
4. delete a dynamically allocated object.
5. temporary objects are destroyed at the end of full expression in which the
   temporary was created.
The destructor is not run when a reference or a pointer to an object goes out
of scope.
By default, the synthesized destructor has an empty function body. members are
destroyed as part of implicit destruction phase that follows the destructor
body.

* The rule of three/five *
There is no requirement that we define all of these operations. However,
ordinarily these operations should be thought of as a unit.
Classes that need destructors need copy and assignment.
Classes that need copy need assignment, and vice versa. But needing either the
copy constructor or the copy-assignment operator does not indicate the need for
a destructor.

* Using = default *
We can explicitly ask the compiler to generate the synthesized version of
copy-control members by defining them as =default;
1. in class body: (become an inline function)
    Sale (const Sale &) = default;
    Sale () = default;
    ~Sale() = default;

2. or outside of class body:
    Sale& Sale:operator=(const Sale&) = default;

* Preventing copies *
 A deleted function is one that is declared but may not be used in any other
way.
NoCopy(const NoCopy&) = delete; // no copy
NoCopy &operator=(const NoCopy&) = delete; // no assignment
~NoCopy() = default; // use the synthesized destructor
The = delete signals to the compiler that we are intentionally not defining
these members.
Unlike = default, = delete must appear on the first declarationof a deleted
function. A default member affects only what code the compiler generates. On
the other hand, the compiler needs to know that a function is deleted in order
to prohibit operations that attempt to use it.
We can only use = default on default constructor, or a copy-control member. But
we can use = delete on any function.

The Destructor should not be a deleted member, otherwise it cannot be destroyed.

* The copy-control member may be synthesized as deleted. *
    If a class has a data member that cannot be default constructed, copied,
    assigned, or destroyed, then the corresponding member will be a deleted
    function.
    If a member has a deleted or inaccessible destructor, it will cause the
synthesized default and copy constructors to be defined as deleted.
    The compiler will not synthesize a default constructor for a class with a
reference member or a const member that cannot be default constructed.
    A class with a const member cannot use the synthesized copy-assignment
operator. It is impossible to assign a new value to a const object.

Private copy control:
prior to new stand, we declare private but not define copy-control functions.
Best practices: Classes that want to prevent copying should define their copy
constructor and copy-assignment operators using = delete rather than making
those members private.

* Copy Control and Resource Management *
We first have to decide what copying an object of an type will mean: valuelike
or pointlike.
Classes that behave like values have their own state. Classes that behave like
pointers share state.
valuelike: library containers, string
pointerlike: shared_ptr
None of the above: IO types and unique_ptr.

Valuelike classes:
Key concept in assignment operators:
1. Assignment operators must work correctly if an object is assigned to itself.
2. Most assignment operators share work with the destructor and copy
   constructor.
A good pattern is to first copy the right-hand operand into a local temporary.
After the copy is done, it is safe to destroy the existing member of the
left-hand operand. Once it is destroyed, copy the data from the temporary into
the memebers of the left-hand operand.
It is important to guard against self-assignment.

Pointerlike classes:
Usually, we can take use of shared_ptr;
But we can also use a reference count.
Also, the copy-assignment operator must handle self-assignment.

* 13.3. Swap *
Algorithm that reorder elements use the class-specific version ot exchange two
elements if a class defines its own, otherwise it uses the swap function
defined by the library.
class HasPtr {
    friend void swap(HasPtr&, HasPtr&);
}
inline void swap(HasPtr &lhs, HasPtr &rhs) {
    using std::swap;
    swap(lhs.ps, rhs.ps);
    swap(lhs.i, rhs.i);
}
Unlike the copy-control members, swap is never necessary. However, defing swap
can be an important optimization for classes that allocate resources.


swap should call swap, not std::swap.
Write type-specific swap version.

Assignment operations that use copy and swap are automatically exception safe
and correctly handle self-assignment.
HasPtr HasPtr::operator=(HasPtr rhs) { // copy by value, not by reference.
    swap(*this, rhs);
    return *this;
}

* A copy-control example *
The library defines versions of swap for both string and set.
swap(set&, set&); swap(string&, string&)

* Classes that manage dynamic memory *
Default-initialization: if T is a class, the default constructor is called; if
it's an array, each element is default-initialized; otherwise, no
initialization is done, resulting in indeterminstic values.
vector has value like behavior.

** Move constructors and std::move **
1. Several of the library classes, including string, define so-called "move
constructors".
2. a library function named `move`, which is defined in the `utility` header.
we call std::move.

The reallocate member
alloc.construct(dest++, std::move(elem++)); // calling move returns a result
that causes construct to use the string move constructor. Each string we
construct will take over ownership of the memory from the string which elem
points. The old strings will no longer manage the memory to which they had
pointed to. We don't know what value the strings in the old ones have, but we
are guaranteed it is safe to run the string destructor on these objects.

### 13.6. Moving objects ###
reasons to move object instead of copying:
1. if an object is immediately destroyed after it is copied. Moving can give it
   a performance boost.
2. IO or unique_ptr can't be coped but can be moved. They can't be shared.

Under the new standard, we can use containers on types that cannot be copied so
long as they can be moved. In previously versions of the library, making a
needless copy can be expensive and classes stored in a container had to be
copyable.

Note:
    The library containers, string, and shared_ptr classes support move as well
    as copy. The IO and unique_ptr classes can be moved but no copied.

* Rvalue references *
To support move operations, the new standard introduced an `rvalue reference`.
rvalue reference may be bound only to an object that is about to be destroyed.
A rvalue reference is a reference that must be bound to an rvalue. &&rvalue

Like any reference, an rvalue reference is just another name for an object.
regular reference is lvalue reference.
We cannot bind a lvalue reference to expressions that requires a conversion, to
literals, or to expressions that return a rvalues.
rvalue reference does the opposite.
int &&rr = i; // error
int $r2 = i * 42; // error: i * 42 is an rvalue.
const int &r3 = i * 42; // ok: we can bind a reference to const to an rvalue.
int &&rr2 = i * 42; // ok.

Functions that return lvalue references, along with the assignment, subscript,
dereference, and prefix increment/decrement operators, are expressions that
return lvalues.
Functions taht returns a nonreference type, along with the arithmetic,
relational, bitwise, and prostfix increment/decrement operators, all yeild
rvalue. We can not bind an lvalue reference to these expressions, but we can
bind either an lvalue reference to const or an rvalue reference to such
expressions.

Lvalue persists; Rvalues are ephemeral.
Lvalue have persistent state, whereas rvalues are either literals or temporary
objects created in the course of evaluating expression.

Variables are Lvaues. we cannot directly bind an rvalue reference to a variable
even if that variable was defined as an rvalue reference type.
int &&rr1 = 42; // literal are rvalue;
int &&rr2 = rr1; // error; rr1 is a lvalue;

int &&rr3 = std::move(rr1); //ok
Calling move tells the compiler that we have an lvalue that we want to treat as
if it were an rvalue. We can destroy a moved-from object and can assign a new
value to it, but we cannot use the value of a moved-from object.
We call std::move not move.

* Move constructor and move assignment *
Differently from the copy constructor, the reference parameter in the move
constructor is an rvalue reference. Any additional parameters must all have
default arguments.
Foo::Foo(Foo &&s) noexcept : // no exception
    el(s.el), first(s.first), cap(s.cap) { // member initializers take over the
resource in s.
    s.el = s.first = s.cap = nullptr; // leave s in a state in which it is safe
to run the destructor. Otherwise, destroying the moved-from object would delete
the memory we just moved.
}

use noexcept to inform the library that the function does not throw any
exception.
Move constructors and move assignment operators that cannot throw exceptions
should be marked as noexcept.

The library container, such as vector, guarantees that if an exception happens
when we call push_back, the vector itself will be left unchanged.

* Move-assignment operator *
Like a copy-assignment operator, a move-assignment operator must guard against
self-assignment.
Foo &Foo::operator=(Str &&rhs) noexcetp{ ...; return *this; }

A moved-from object must be destructible
The moved-from object is safe to destroy, and it is also valid to give a new
value. On the otherhand, there is no requirements as to the remaining value.
e.g., if we move from a string or a container, the moved-from object is still a
valid object, with all the attributes, but we don't know the remaining value.

After a move operation, the "moved-from" object must remain a valid,
destructible object but users may make no assumption about its value.

* The synthesized move operations *
Conditions:
1. the compiler will synthesized a move constructor or a move-assignment operator
only if the class doesn't define any of its own copy-control member and if
every every non-static data membe of the class can be moved.
2.  The compiler can move members of built-in type. It can also move members of
    a class type if the member's class has the corresponding move operation.

A move operation is never implicitly defined as a delted function.
The rulef for when a synthesized move operation is defined as deleted:
1. if the class has a member that defines its own copy constructor but does not
   also define a move constructor, or if the class has a member that doesn't
define its own copy operations, but for which the complier is unable to
synthesize a move constructor.

A move operation is never implicitly defined as a delted function.
The rulef for when a synthesized move operation is defined as deleted:
1. if the class has a member that defines its own copy constructor but does not
   also define a move constructor, or if the class has a member that doesn't
define its own copy operations, but for which the complier is unable to
synthesize a move constructor.

A move operation is never implicitly defined as a delted function.
The rulef for when a synthesized move operation is defined as deleted:
1. if the class has a member that defines its own copy constructor but does not
   also define a move constructor, or if the class has a member that doesn't
define its own copy operations, but for which the complier is unable to
synthesize a move constructor.
2. the class has a member whose own move constructor or move-assignment
   operator is deleted or inaccessible.
3. the move constructor is defined as deleted if the destructor is deleted or
   inaccessible.
4. the move-assignment operator is defined as deleted if the class has a const
   or reference member.

Classes that define a move constructor or move-assignment operator must also
define their own copy operator. Otherwise, those synthesized membes are deleted by default.

* Rvalues are moved, Lvalues are copied..*
StrVec v1, v2;
v1 = v2; // v2 is an lvaue; copy assignment;
StrVect getVect(istream &);
v2 = getVect(cin); // move assignment; both matches. But calling the
copy-assignment operator requires a conversion to const, where StrVect&& is an
exact match.

* But Rvalues are copied if there is no move constructor *
Foo z(std::move(x)); // copy constructor will be calld, because there is no
move constructor. the copy constructor for Foo is viable becaue we can convert
a Foo&& to a const Foo&.
Using the copy constructor in place of a move constructor is almost surely safe.

*Copy-n-swap assignment operators and move *
HasPtr& operator=(HasPtr rhs){}; //the paremeter is copy initialized; depending
on the type of the argugment, copy initialization uses either the copy
consructor or the move constructor.

All five copy-control members should be thought of as a unit: Ordinarily,if a
class defines any of these, it usually should define them all.

foldes = std::move(m->folders); // use set move assignment.

* move iterator *
unitialized_copy(make_move_iterator(begin()), make_move_iterator(end), first);

Advice: Don't be too quick to move
Because a moved-from object has indeterminate state, calling std::move on an
object is a dangerous operation. When we call move, we must be absolutely
Judiciously used inside class code, move can offer significant performance
benefits.
Outside of class implementation code such as move constructors or
move-assignment operators, use std::move only when you are certain that you
need to do a move and that the move is guaranteed to be safe.

* Rvalue references and member functions *
Member functions other than constructor and assignment can benefits from
providing both copy and move version.
void push_back(const X&); // copy: binds to any kind of X. Free to copy from
its parameter.

Note: overloaded functions that distinguish between moving and copying a
parameter typically have one version that takes a const T& and one that takes a
T&&.
void push_back(X&&); //move: binds only to modifiable rvalues of type X. Free

void StrVec::push_back(string &&s) {
    chk_n_alloc();
    alloc.construct(first_free++, std::move(s)); // construct takes a rvalue
refrence as the second parmameter in order to use the move constuctor, but the
s is assigned to the second parameter of constructor and s becomes a lvalue in
the right side of assignment, so we must call std::move(s) to get a rvalue
reference.
}
to steal source from its parameter.
certain that there can be no other users of the moved-from object.

* Rvalue and Lvalue reference member function *
We indicate the lvalue/rvalue property of this in the same way that we define
const member functions:
Foo &operator=(const foo&) &; // may assign only to modifiable lvalue;
& or && indicates this may point to an rvlaue or lvalue.
Like the const qualifer, a reference qualifer may appear only on a (nonstatic)
member function and must appear in both the declaratin and definition of the
function.
Foo another() const &; // const before &

* Overloading and reference functions *
If a member function has a reference qualifier, all the versions of that member
with the same parameter list must have reference qualifier.

Warning:
Foo A(); // oops! declare a function, not an object
Foo A; // A is an initialized object. No trailing parenthese.


## 14. Overloaded operations and conversions
Except for the overloaded function-call operator, operator(), an overloaded
operator may not have default arguments.
When an overloaded operator is a member function, this is bound to the
left-hand operand, hence one parameter less.
A overloaded operator has the same precedence and associativity.

data1 + data2;
operator+(data1, data2); //equivalent
data1.operator+=(data2); // member function

Ordinarily, the comma, address-of, logical AND, and logical OR operators should
not be overloaded.

Use definitions that are consistent with the built-int meaning.

Caution: use operator overloading judiciously:
e.g., string uses + to represent concatenation.

Choosing member or Nonmember implementation
1. The=, [], () and -> operator must be defined as members;
2. the compound-assignment operators normally ought to be members, but not
   required.
3. Operators taht change the state of their object or that are closely tied to
   their given type-such as increment, decrement, and dereference, usually
   should be members.
4. Symmetric operators-those that might convert either operand, such as the
   arithmetic, equality, ralational, and bitwise operators- usually should be
   defined as nonmember functions.

"hi" is a const char \*, which is a builtint type.

* overloading the output operator <<
ostream &opeator<<(ostream &os, const Sales_data &item) { return os;}

IO operators are non-member functions, and they must be declared as friends.

Input operators should decide what, if anything, to do error recovery.

Arithmetic and relational operators

* Equality operators *
bool operator==(const Sale_data &lhs, const Sale_data &rhs){};
Classes for which there is a logical meaning for equality normally should
define operator==. Classes that define == make it easier for users to use the
class with the library algorithms.

Relational operators:
If a single logical definition for < exists, classes ususally should define the
< operator. However, if the class also has ==, define < only if the definition
of < and == yield consist results.

* Assignment operators *
In addition to the copy- and move-assignment operators that assign one object
of the class type to another of the same type, a class can define additional
assignment operators that allow types as the right-hand operand.
e.g. vector<string> v;
v = { "a", "b", "c"};
StrVec &operator=(std::initializer_list<std::string>);

Assignment operators must, and ordinarily compound-assignment operators should,
be defined as memembers. These operators should return a reference to the
left-hand operand.

* Subscript Operator *
If a class has a subscript operator, it usually should define two versions: one
that return a plain reference and the other that is a const member and return a
reference to const.

Non const object can invoke const or non const member function, while const
object can only invoke cont member function.

* Increment and Decrement Operators *
Classes that define increment or decrement operators should define both. And
they are defined as members.
StrBlobPtr operator++(int); //postfix; the int parameter is not used, so we do
not give it a name.
StrBlobPtr operator++(); //prefix

Operator arrow must be a member. The dereference operator is not required to be
a member but usually should be a member as well.

The overloaded arrow operator must return either a pointer to a class type or
an object of a class type that defines its own operator arrow.

* Function-call Operator *
int operator()(int val) const{};
The function-call operator must be a member function. A class may define
multiple versions of the call operator, each of which must differ as to the
number or types of their parameters.

Objects of class taht defines the call operator are referred to as "function
objects".
for_each(vs.begin(), vs.end(), PrintString(ceer, '\n'));
Function objects are most often used as arguments to the generic algorithms.

* Lambdas are function objects *
When we write a lambda, the compiler translate that epression into an unnamed
object of an unmaed class.

Classes representing lambdas with captures:
If captured by value, those arguments will be saved to data memembers of the
generated lambda class.

* Library-defined function objects *
A set of class in the stdlib that represent the arithmetic, relational, and
logical operators defines a call operator that applies the named operation.
e.g., "plus" applies to "+".
And those are templates.
Plus<int>   or Plus<string>
modules: %
equal_to: ==

Using a library function object with the algorithms
sort(svec.begin(), svec.end(), greater<string>()); // by default using
operator<(), which gives an ascending order. Now it gives a descending order.

One important aspect of these library function objects is that the library
guarantees that they will work for pointer, while comparison on two unrelated
pointers is undefined.

The associative containers use less<key_type> to order their elements.

* Callable Objects and function *
Several kinds of callable objects:
1. function 
2. function pointers.
3. lambdas
4. objects created by bind
5. class that overloads function-call operator.
Different types can have the same call signature, but they are essentially
different types.
Using function in functional header to solve the problem:
function<int(int,int)> f1 = add;
map<string, function<int (int, int)>> binops;
int (*fp) (int, int) = add;
binops.insert({"+, fp"}); // to solve the overloaded function issue.

* Overloading, conversions, and operators*
A constructor that can be called with a single argument defines an implicit
conversion from the constructor's parameter type to the class type. Such
constructors are referred to as converting constructors.

Conversion operator defines conversion from one class type to another class
type.

Converting constructors and conversion operators are called user-defined
conversions.
Conversion operator: operator type() const;

A conversion function must be a member function, may not specify a return type,
and must have an empty parameter list. The function usually should be const.

SmallInt si;
si = 4; // implicitly convert 4 to SmallInt then calls SmallInt::operator=
si + 3; // implicitly convert si to int followed by integer addition
si +3.24;
SmallInt si = 3.2; // two conversions. double to int, int to SmallInt(int);

Avoid overuse of the conversion functions.

Conversion operators can yield surprising results

cin << 42; // legal if the conversion to bool were not explicit.
Because bool is an arithmetic, a class-type object that is converted to bool
can be used in any context where an arithmetic is expected.

explicit operator int() const { return val; }
si + 3; // error
static_cast<int>(si) + 3; // explicitly request the conversoin.

if an expression is used as a condition, an explicit conversion will still be
applied by the compiler.

Best: conversion to bool is usually intended for use in condition. As a result,
operator bool ordinarily should be defined as explicit.

Avoid ambiguous conversion. It is a bad idea to define classes with mutual
conversions or to define conversions to or from two arithmetic types.

Needing to use a constructor or a cast to convert an argument in a call to an
overloaded function frequently is a bad design.

The set of candidate functions for an operator used in an expression can
contain both nonmember and member functions.

Providing both conversion functions to an arithmetic type and overloaded
operators for the same class type may lead to ambiguities between the
overloaded operators and the built-in operators.

## Chapter 15. Object-Oriented Programming
Inheritance: base class, derived class.
The base class defnies as virtual those functions it expects its derived
classes to define for themselves.

Dynamic binding is known as run-time binding. The decision can't be made until
run time.
In C++, dynamic binding happens when a virtual function is called through a
reference (or a pointer) to a base class.
Base classes ordinarily should define a virtual destructor. Virtual destructors
s are needed even if they do not work.

The derived class needs to override the definition it inherits from the base
class.
A base class specifies that a member function should be dynamically bound by
preceding its declaration with the keyword `virtual`. Any nonstatic member
function, other than constructors, may be a virtual. The virtual keyword
appears only on the declaration inside the class. A function declared as
virtual in the base class is implicitly virtual in the derived classes as well.
non virtual function is resolved at compile time.

Protected access control: accessiable by derived classes, but not others.

* defining a derived class
class derived_class : access_qualifier base_class {}

The the deviation is public, the public members of the base class become part
of the interface of the derived class as well. In addition, we can bind an
object of a publicly derived type to a pointer or reference to the base type.

single inheritance and multiple inheritance.

Derived class frequently, but not always, override the virtual functions that
they inherit. The new standard lets a derived class explicitly to put a
"overrite" keyword after.

A derived object contains multiple parts: a subobject containing the nonstatic
members defined in the derived class, plus subobjects corresponding to each
base class from which the derived class inherits.
The base and derived parts of an object are not guranteed to be stored
contiguously.

Quote *p = &item;
Quote &r = bulk;
Binding a reference or a pointer to the base-part of a derived object:
derived-to-base conversion.
The fact that a derived object contains subobjects for its base classes is key
to how inheritance works. It even contains the private part of the base class,
though not able to access it directly.

Each class controls how its member are initialized. A derived class cannot
directly initialize the member of its base class. It must use a base-class
constructor to initialize its base-class part.
Bulk_quote(...):Quote(..), a(c) {}
The base class is initialized first, and then the members of the derived class
are initialized in the order in which they are declared in the class.

The scope of a derived class is nested inside the scope of its base class.

Key concept: respecting the base-class interface
Even though the derived class can assign to the public or protected base-class
members of its base class, it should not do so, instead of using a constructor.

Static members are inherited, if not private. We can access it in multiple
ways: base/derived class scope accessor, object member accessor, or implicit
access.

The declaration cantains the class name but does not include its derivation
list.
Class Bulk_quote;
But a class must be defined, not just declared, before we can use it as a base
class.
Inheritance chain: the most derived object contains a subobject for its direct
base and for each of its indirect bases.

class NoDerived final {} // prevent inheritance.
class Last final : Base{}

The static type of a pointer or reference to a base class may differ from its
dynamic type.
static type is the type with which a variable is declared or that an expression
yields.
We cannot convert from base to derived when a base pointer or reference is
bound to a derived object, because the compiler has no way to know it.
If the base class has one or more virtual functions, we can use a dynamic_cast.
If we know it is safe, we can use static_cast.

The automaitc derived-to-base conversion applies only for conversion to a
reference or pointer type. There is no such conversion from a derived-class
type to the base-class type.

When we initialized or assign an object of a base type from an object of a
derived type, only the base-class part of the derived object is copied, moved,
or assigned. The derived part of the object is sliced down.

There is No implicit conversion from base to derived.

* Virtual Functions*
Ordinarily, if we do not use a function, we don't need to supply a definition
for that function. However, we must define every virtual function, regardless
of whether is is used, because the compiler has no way to determine whether a
virtual function is used.

Call to virtual functions May Be resolved at run time:
Virtuals are resolved at run time only if the call is made through a reference
or pointer. Only in these case is it possible for an object's dynamic type to
differ from its static type.

When a derived class overrieds a virtual function , it may, but is not required
to, repeat the virtual keyword. Once a function is declared as virtual, it
remains virtual in all the derived classes.
Except for return type, a virtual function must have exactly the same parameter
types as the base-class function that it overrides.

It is legal for a derived class to define a function with the same name as a
virtual in its base but with a different parameter list. The compiler will
consider it as a different version.
Using override to enlist the compiler in finding errors about virtuals.

void f1(int) const final; //subsequent classes can't override it.

Virtual function and default arguments
When a call is made through a reference or pointer to base, the default
arguments will be those defined in the base class. The derived function will be
passed the default arguments defined for the base-class version of the
function.
So virtual functions that have default arguments should use the same argument
values in the base and derived class.

baseP->Quote::net_price(11); //cicumveting the virtual mechanism
Ordinarily, only code inside member functions (or friend) should need to use
the scope operator to circumvent the virtual mechanism.

* Abstract Base Class *
double net_price(std::size_t) const = 0; // a pure virtual function.
We can provide a definition for a pure virtual function, but do not have to.
On the contrary, we have to define a virtual function.

Classes with a pure virtual function are abstract base classes.

**Access control and inheritance**
protected members, like private, are inaccessiable to users of the class, and
like public, are accessible to members and friends of classes derived from this
class.
But a derived class member or friend may access the protected member of the
base class only through a derived object.

The derivation access specifier has no effect on whether members (and friends)
of a derived class may access the member of its own direct base class.
The purpose of the derivation access specifier is to control the access that
users of the derived class-including other classes derived from the derived
class-have to the members inherited from Base.

Accessibility of Derived-to-Base conversion
For any given point in your code, if a public member of the base class would be
accessible, then the derived-to-base conversion is also accessible, and not
otherwise.
A class that is used as a base class makes its interface member public. An
implementation member should be protected if it provides an operation or data
that a derived class will need to use in its own implementation. Otherwise,
they should be private.

Friendship is not transitive.
Friendship is not inherited; each class controls access to its members.

Exempting individual members:
public:
    using Base::n;
A derived class may provide a using declaration only for names it is permitted
to access.

By default, a derived class defined with class keyword has private inheritance;
a derived class defined with struct has public inheritance.

**Class Scope under inheritance**
The scope of a derived class nests inside the scope of its base classes.
Name lookup happens at __compile time__.
The static type of an object, reference, or pointer determines which members of
that object are visible.
Like any other scope, a derived-class member with the same name as a member of
the base class hides direct use of the base-class member.

Using the scope operator to use Hidden Members

Key concetp: name lookup and inheritance
p->mem()
1. first determine the static type of p.
2. look for mem in the p class. If not find, look in the inheritance chain.
3. once mem is found, do normal type checking.
4. the compiler generates code, depending on whether it is virtual or not:
    - if virtual and the call is made through a reference or pointer, then the
      compiler generates code to determine at run time which version to run
      based on the dynamic type of the object.
    - Otherwise, the compiler generates a normal function call.

**As usual, name lookup happens before type checking**
As in any other scope, if a member in a derived class has the same name as a
base-class member, then the derived member hides the base-class member within
the scope of the derived class. The base member is hidden even if teh functions
have different parameter lists. There is no overloading in the scope.
d.Base::memfnc() to skip the hidding.

Overriding overloaded functions
If a derived class wants to make all the overloaded versions available through
its type, then it must override all of them or none of them.
using function names adds all the overlaoded instances of that function to the
scope of the derived class, and the derived class needs to define only those
functions truly depend on its type. It can use the inherited definitions for
the others.

**Constructors and Copy Control**

// virtual destructor needed if a base pointer pointing to a derived object is
deleted.
virtual ~Quote() = default; // dynamic binding for the destructor
Quote *p = new Bulk_Quote;
delete p;  //destructor for Bulk_Quote is called.

Executing delete on a pointer to base that points to a derived object has
undefined behavior if the base's destructor is not virtual.
Once a function is virtual, the corresponding function in its derived class is
implicitly virtual.

If a class defines a destructor- even if it uses = default to use the
synthesized version - the compiler will not synthesize a move operation for
that class.

Base classes and deleted copy control in the derived.

Default copy contorl chain in the inheritance chain.

B(const B&) = deleted; // B won't have a synthesized move constructor.

Because lack of a move operation in a base class suppress synthesized move for
its derived classes, base classes should ordinarily define the move operations.

When a derived class defines a copy or move operation, that operation is
responsible for copying or moving the entire object, including base-class
members.

By default, the base-class default constructor initialize the base-class part
of a derived object. If we want copy (or move) the base-class part, we must
explicitly use the copy(or move) constructor for the base class in the
derived's constructor initializer list.

The derived class assignement oerpator must assign its base part explicitly.
Unlike the constructors and assignment operators, a derived destructor is
responsbile only for destroying the resources allocated by the derived class.
Object are destructed in the opposite order from which they are constructed:
the derived destructor is called first.

Note: if a constructor or a destuctor calls a virtual, the version that is run
is the one corresponding to the type of the constructor or destructor itself.

**Inherited Constructor**
class Bulk_quote : public Disc_quote {
public:
    using Disc_quote::Disc_quote; //inherit Disc_quote's constructors
}
Ordinarily, a using declaration only makes a name visible in the current scope.
When applied to a constructor, a using declaration causes the compiler to
generate code: derived(params) : base(args) {}.
A using declaration can't specify explicit or constexpr, but it can inherite
access qualifer and explicit and constexpr. But the default arguments cannot be
inherited.

**Containers and Inheritance**
Because derived objects are "sliced down" when assigned to a base-type object,
containers and types related by inheritance do not mix well. When we can put
derived class object in the container, it will be sliced and copied into the
container.

So Put (Smart) pointer, not objects, in containers.
Just as we can convert a ordinary pointer to a derived type to a base-class
type, we can also conert a smart pointer to a derived  type to a smart pointer
to an base-class type.

virtual Quote* clone() const &{ return new Quote(*this);}
virtual Quote* clone() && { return new Quote(std::move(*this));} // clone is
overloaded based on whether it is called on an lvalue or an rvalue.

In C++, dynamic binding appies only to functions declared as `virtual` and
called through a reference or pointer.
virtual Quote* clone()

## Chapter 16. Templates and Generic Programming
OOP deals with types that are not known until run time, whereas in generic
programming the types become known duing compilation.

**Function Template**
template <typename T>
int comapre(const T &v1, const T v2) {}

A templaet definition starts with the keyword template followed by a `template
parameter list` which can not be emtpy.
template instantiation
Each type parameter must be preceded by the keyword `class` or `typename`.
Template arguments used for nontype template parameters must be constant
expressions.

When we call a function template, the compiler uses the arguments of the call
to deduce the template argument(s) for us.

template <typename T> inline T min(const T&, const T&);
The inline or constexpr can be used in a function template.
Template programs should try to minimize the number of requirements placed on
the argument types.

Defintions of function templates and member functions of class templates are
ordinarily put into header files.
Ordinarily, when we call a function, the compiler needs to see only a
declaration for the function, but the definition. As a result, we put class
defintions and function declarations in header files and defintion of ordinary
and class-member functions in source files.

It is up to the caller to guarantee that the arguments passed to the template
support any operations that templates uses, and tha those operations behave
correctly in the context in which the template uses them.

The code is not generated until a template is instantiated.

**Class Templates**
template <typename T> class Blob {}
Each instantiation of a class constitues an independent class.
As with any class, we can define the member of a class tempalte either inside
or outside of the class body. As with any other class, members defined inside
the class body are implicitly inline.

template <typename T>
return-type Blob<T>::member-name(param-list)

We can simplify use of a template class name inside class code.
Inside the scope of a class template, we may refer to the template without
specifying template arguments.

*Class Templates and Friends*
In order to refer to a specific instantiation of a template, we must first
declare the template itself.
template <typename> class BlobPtr;

General and specific template friendship:
template <typename T> class Pal;
class C {
    template <typename T> friend class Pal2;// no forward declaration needed
when we befriend all instantiation;
    firend class Pal<C>;
};

Befriending the Template's own type parameter

Template type aliases(two forms):
typedef Blob<string> strBlob;
template<typename T> using twin = pair<T, T>;
template<typename T> using partNo = pair<T, unsigned>;

static members of class template:
template <typename T> class Foo {
    private:
        static std::size_t ctr;
}
Foo<int> f1, f2, f3; // all share the same ctr;
template <typename T> size_t ctr = 0; // define and initialize

*Template parameters and scope*
We cannot reuses names of templates parameters in the template. A parameter
name cannot be resued.
As with function parameters, the names of a template parameter need not be the
same across the declarations and the definition of the same template.

*Using class members that are types*
We can use the scope operator :: to access both static members and type members.
template <typename T>
typename T::value_type top(const T& c); // when we wan to inform the compiler
that a name, e.g., value_type, represents a type, we must use the keyworkd typename, not class.
Because the compiler doesnot know whether T::mem is a static member or a type.

*Default template arguments*
template <typename T, typename F = less<T>>
int compare(const T &v1, const T &v2, F f = F()){}

*Tempalte Default arguments and class templates*
Whenever we use a class template, we must always follow the template's name
with brackets.
template <class T= int> class Numbers{};
Numbers<> foo_number;

**Member Templates**
*member template*
class Foo {
    public:
        template <typename T> void operator()(T *p) const { delete p;}
}
member templates of class tempaltes:
When we define a member template outside the body of a class template, we must
provide the template parameter list for the class template and for the function
template.
template <typename T>
template <typename It> Blob<T>::Blob(It b, It e):data(std::make_shared<std::vector<T>>(b,e)) {}

*Constrolling instantiation*
To avoid the overhead of instantiating the same template in multiple files, we
can use explicit instantiation.
extern template declaration; // instantiation declaration
template declaration; // instantiation declaration

etc:
extern template class Blob<string>; //declaration
template int compare(const int&, const int&); // definition
There may be several extern declarations for a given instantiatin but there
must be exactly one defition for that instantiation.

Efficiency vs flexibility:
run time deleter for shared_ptr;
compile time deleter for unique_ptr.

**Template Argument Deduction**
the process of determining the template arguments from the function arguments
is known as template argument deduction.

*conversion and template type parameters*
As usual, top-level consts in either the parameter or the argument are ignored.
The only other conversions performed in a call to a function template are:
1. const conversions: A function parameter that is a reference or pointer to a
const can be passed a reference or pointer to a non-const object.
2. Array or function-to-pointer conversions: if the function parameter is not a
reference type, then the normal pointer conversion will be applied to arguments
of array or function type.
Other conversions, such as the arithmetic conversions, derived-to-base, and
user-defined conversions, are not performed.

Function parameters that use the same template parameter type.
template <typename A, typename B>
int flexibleCompare(const A& v1, const B& v2){}
argument types can differ but must be compatible.

Normal Conversion still Applies for Orindary Arguments

Function-Template explicit arguments
template <typename T1, typename T2, typename T3>
T1 sum(T2, T3);
auto val3 = sum<long long>(i, lng); // specify explicitly T1.
match from left to right.

Normal conversion apply for explicitly specified arguments

*Trailing return types and type tranformation*
template <typename t>
auto fcn(It beg, It end) -> decltype(*beg){}
// if beg is called on an iterator of strings, it will return string&.

decltype((variable)) is always a reference type, but decltype(variable) is a
reference type only if variable is a reference.
When we apply decltype to an expression(other than a variable), the result is a
reference type if the expression yields a lvalue.
decltype(*p) is int &, assuming p is int*.
decltype(&p) is int**, a pointer to a pointer to an int.

*The type transformation library template classes*
defined in the `type_traits` header.
In general the classes in type_traits are used for so-called template
metaprogramming.
auto fcn2(It beg, It end) -> typename remove_reference<decltype(*beg)>::type
{};
Each tempalte has a public member named `type` that represents a type.

*Function pointers and argument Deduction*
template <typename T> int compare(const T&, const T&);
int (*pf1) (const int&, const int&) = compare;
When the address of a function-template instantiation is taken, the context
must be such that it allows a unique type or value to be determined for each
template parameter.

Template Argument Deduction and References
Type deduction from Lvalue reference function parameters.
const int& a = 20; // OK; a temporary int is created and its life is extended.
int& b = 10; // error

Type deduction from rvalue reference function parameters
template <typename T> void f3(T&&);
f3(42);

*Reference Collasping and Rvalue Reference Parameters*
f3(i); // legal exception
Tow exceptions to normal binding rules that allow this kind of usage. These
exception are the foundation for how library facilities such as move operate.
1. when f3(i) is called on f3(T&&), the compiler deduces the type of T as int&,
not int, the argument's lvalue reference type.
2. Oridinarily, we cannot directly define a reference to a reference. In all
but one case, the references collapse to form an ordinary lvalue reference
type. X& &, X& &&, X&&, & all -> X&, while X&& && -> X&&.
Reference collasping applies only when a reference to a reference is created
indirectly, such as in a type alias or a template parameter.

Reference collasping applies `only` when a reference to a reference is created
indirectly, such as in a type alias or a template parameter.

Note: an argument of any type can be passed to a function parameter that is an
rvalue reference to a template parameter type(T&&). When an lvalue is passed to
such a parameter, the function parameter is instantiated as an ordinary, lvalue
reference(T&).
It's worth noting that function templates that use rvalue references often use
overloading:
template <typename T> void f(T&&); // bind to modifiable rvalues
template <typename T> void f(const T&); // lvalues and const rvalue

*Understanding std::move*
How move works:
template <typename T>
typename remove_reference<T>::type&& move(T&& t) {
    return static_cast<tyepname remove_reference<T>::type&&>(t);
}
string s1("hi"), s2;
s2 = std::move(string("bye!")); // moving from an rvalue;
s2 = std::move(s1); //ok, but after assignment s1 has indertermine value.

static_cast from an Lvalue to an Rvalue reference is permitted
Binding an rvalue reference to an lvalue gives code that operate on the rvalue
reference permission to clobber the lvalue.

*Forwarding*
A function parameter that is an rvalue reference to a template type parameter
(T&&) preserves the constness and lvalue/rvalue property of its corresponding
argument.
When used with a function parameter that is an rvalue reference to template
type parameter(T&&), `forward` preserves all the detail about an argument's
type.
template <typenamt F, typename T1, typename T2>
void flip(F f, T1 &&t1, T2 &&t2) {
    f(std::forward<T2>(t2), std::forward<T1>(t1));
}

**Overloading and Templates**
When there are several overloaded templates that provides an equally good match
for a call, the most specialized version is preferred.
When a nontemplate function provides an equally good match for a call as a
function template, the nontemplate version is preferred.

*Variadic templates*
A variadic templates is a template function or class that can take a varying
number of parameters. And parameter pack.
template <typename T, typename... Args>
void foo(const T &t, const Args& ... rest);
sizeof...(args)

Variadic functions are often recursive.

Pack expansion
debug_rep(rest)... // call debug_rep on each member of rest parameter pack.

Forwarding parameter packs
The emplace_back member of the library container is a variadic member template
that uses its arguments to construct an element directly in space managed by
the container.

std::forward<Args>(args)... // forward to preserve the type information
template <typename... Args>
void fun(Args&&... args) {
    work(std::forward<Args>(args)...);
} // expands both the template parameter pack and the function parameter pack.
Because the parameter to fun are rvalue references, we can pass arguments of
any type to fun; because we use std::forward to pass those arguments, all type
information about those arguments will be preserved in the call to work.

**Template Specialization**
template<> //<> inciates that arguments will be supplied for all the template
parameters of the original template.
template<>
int compare(const char* const &p1, const char* const &p2) {
    return strcmp(p1, p2);
}
Templates and their specializations should be declared in the same header file.
Declarations for all the templates with a given name should be appear first,
followed by any specifializations of those templates.

specialization instantiate a template; they do not overload it. As a result,
specialization do not affect function matching.

*class template specializations*
namespace std {
template<>
    struct hash<Sale_data> {}
} // open the namespace
We can patially specialize only a class template. We cannot partially
specialize a function template.

*Summary*
The library algorithms are function templates and the library containers are
class templates.

## Specialized Library Facilities
*tuple*
tuples can have any number of members.
tuple<T1,T2,...,Tn> t(v1, v2, v3,...., vn);
make_tuple(v1,v2,...,vn)
get<0>(item); // return a reference to the first item member.
Because tuple defines the < and == operators, we can pass sequences of tuples
to the algorithms and can use a tuple as key type in an ordered container.

*bitset*
bitset<n> b; // initialize with ULL or a string
operations on bitsets.

*Regular expression*
regular expression library components
regex (and wregex) operations
The regular expression is not interpreted by the C++ compiler. The syntactic
correctness of a regular expression is evaluated at run time.

*Random Numbers*
C++ program should not use the library rand function. Instead, they should use
the default_random_engine along with an appropriate distribution object.
uniform_int_distributed<unsigned> u(0,9);
default_random_engine e;
cout << u(e) << " ";

A given random-number generator always produces the same sequence of numbers. A
function with a local random-number generator should make that generator(both
the engine and distribution objects) static. Otherwise, the function will
generate teh identical sequence on each call.

Using time as a seed usually doesn't work if the program is run repeatly as
part of an automated process; it might wind up with the same seed several
times.
default_random_engine e1(time(0));

*IO library*


## Tools for large programs
**Exception handling**
Stack unwinding
An exception that is not caught terminate the program.

Objects are automatically destoryed during stack unwinding.
During stack unwinding, destructors are run on local objects of class type.
Because destructors are run automatically, they should not throw. If, during
stack unwinding, a destructor throws an exception that it does not also catch,
the program will be terminated.

The compiler manages the exception object space.
Throwing a pointer requires that the object to which the pointer points exist
wherever the corresponding handler resides.

*Catching an exception*
In exception declaration,
We can omit the name of the catch parameter if the catch has no need to access
the thrown expression.
Ordinarily, a catch that takes an exception of a type related by inheritance
ought to define its parameter as a reference.
Multiple catch clauses with types related by inheritance must be ordered from
most derived type to least derived.

Using 'throw' to rethrow 
An empty throw can appear only in a catch or in a function called from a catch.

The Catch-All Handler: catch (...) // matches any type of excpetion
catch(...) is often used in combination with a rethrow expression.

Function try blocks and constructors
template <typename T>
Blob<T>::Blob(std::initializer_list<T> li) try:
data(std::make_shared<std::vector<T>(i1)) {} catch() {}

void recoupt(int) noexcept; // won't throw
Either the function won't throw, or the whole program will terminate. The
caller escapes responsibility either way.

void f() noexcept(noexcept(g())); // f has same exception specifier as g.

Hiearchy:
1. exception:
    - bad_cast
    - runtime_error: overflow_error, underflow_error, range_error
    - logic_error: domain_error, invalid_argument, out_of_range, length_error
    - bad_alloc
The only operations taht the exception types define are the copy constructor,
copy-assignment operator, a virtual destructor, and a virtual member named
what.
The exception, bad_cast, and bad_alloc also define a default constructor. The
runtime_error and logic_error do not have a default constructor but do have
constructors that take a C-style cstring or a library string.

**Namespaces**
A namespace is a scope.

namespace Foo {
    class A {};
    B operator+(const Sale_data&, const Sale_data&);
}
Any declarations that can appear at global scope can be put into a namespace:
classes, variable(with their initializations), function(with their
definitions), templates, and other namespaces.

Names defined in a namespace may be accessed directly by other members of the
namespace, including scope nested within those members.

Namespaces can be discontinguous
namespace nsp{} either defines a new namespace or adds to an existing one.

1. Namespace members that define classes, and declaration for the functions and
object that are part of the class interface, can be put into header files.
These headers can be included by files that use those namespaces members.
2. The definitions of namespace member can be put int separate source files.

Namespaces that define multiple, unrelated types should use seperate files to
represent each type(or each collection of related type) that the namespaces
defines.

Ordinarily, we do not put a #include inside the namespace.

Defining Namespace members
It is possible to define a namespace member outside its namespace definition.

The Global namespace
The global namespace is implicitly declared and exists in every program.
::member_name // access a global member

Nested Namespaces

inline namespaces
inline namespace FifthEd{

}

Unnamed Namespaces is local to a particular file and never spans multiple
files.
In C, a global entity declared `static` is invisible outside the file in which
it is declared.

**Using namespace members**
namespace primer = cplusplus_primer;
namespace Qlib = aaa:bbb:QueryLib;

A using declaration introduces only one namespace member at a time.
A using declaration can appear in global, local, namespace, or class scope. In
class scope, such declarations may only refer to a base class member.

using directives:
using namespace foo; // makes all the names from foo visible.

using declaration only makes the name directly accessbile in the local scope.
In contrast, a using directive causes all the names appearing in the enclosing
namespace scope.

The one place where using directives are useful is in the implementation files
of the namespace itself.

Argument-dependent lookup and parameters of class type
When we pass an object of a class type to a function, the compiler searches the
namespace in which the argument's class defined in addition to the normal scope
lookup.

lookup and std::move and std::forward
