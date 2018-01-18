# Scala for the impatient
## The basic
1.1 Scala is unusual because it is usually installed for each of your Scala projects rather than system wid.
sbt console: enter scala's REPL3.
Technically speaking, the `scala` is not an interpreter. Behind the scenes, your input is quickly complied into bytecode, and the bytecode is executed by the Java virtual machine.

1.2 declaring values and variables
val ans = 2 // a constant
var change = 4 // a variable
You need not specify the type of a value or variable. It is inferred, but it is an error to declare a variable or value without initializing it.
val a, b: Int = 1,2 // specify explicitly.

1.3 Commonly used types
Byte, Char, Short, Int, Long, Float and Boolean.
They are all classes. There is no distinction between primitive and class types in Scala.
etc, 1.toString() // 1
Scala augments the java String class with well over a hundred operations in the `StringOps` class.
"Hello".intersect("World") // "lo"
Similarly, there are class `RichInt`, `RichDouble`, `RichChar`. Each of them has a small set of convenience methods for acting on their poor cousins-- `Int`, `Double`, `Char`.
`BitInt`, `BigDecimal` for computations with an aribitrary number of digits, backed by java.math.BigInteger and java.math.BigDecimal.
"99.44".toDouble // 99.44

1.4 Arithmetic and operator overloading.
a + b is a shorthand for a.+(b)
These operators are actually methods, just as python and ruby.
a method b as a shorthand for a.method(b)
There is no self increment or decrement. counter += 1
You can use the usual mathematical operators with BigInt and BigDecimal.
val x: BigInt = 2123132321312
x * x * x

1.5 More about calling methods
If the method has no parameters, do not have to use parentheses.
"Bonjour".sorted // "Bjnooru"
The ruel of thumb is that a parameterless method that doesn't modify the object has no paretheses.
import packageName._ whenever you need to import a particular package._

1.6 The apply method.
"hello"(4) // 'o'
`()` is implememted as a method with the name `apply`.
s(4) is a shortcut for s.apply(4)
Big("1234456") is a shortcut for BigInt.apply("12314455").
Use the `apply` method of a companion object is a common scala idioms for constructing objects.

1.7 More
Methods can have functions as parameters:
def count(p:(Char) => Boolean): Int
classes such as `Range` or `Seq[Char]`.
Use square bracktes for type parameters. Seq[Char]
