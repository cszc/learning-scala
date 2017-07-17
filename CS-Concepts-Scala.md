# Programming Concepts in Scala
**July 2017**

This collection of notes serve as an outline of concepts needed to learn Scala. Specifically, these are the topics I found to be confusing with only an intermediate knowledge of Python. Hopefully, they will be useful to other people with a limited knowledge of programming concepts.

-------

## Anonymous Functions
*TBD*

## Data Structures

### Collections
*TBD*
- List
    - http://alvinalexander.com/scala/how-create-scala-list-range-fill-tabulate-constructors
- Vector
- ListBuffer
- IndexSeq

## Functional programming
*TBD*

### *Fun*-ctional Methods
- Fold
    - e.g. [Find the number of elements of a list](http://blog.thedigitalcatonline.com/blog/2015/04/07/99-scala-problems-04-length/#.WWk4VBPyvOR)
- Reduce
- Scan

## Implicit Returns
- [Don't Use Return in Scala](https://tpolecat.github.io/2014/05/09/return.html)

## Idiomatic Scala
    - tail, head

## Infix, Prefix, & Postfix Operators in Scala

**TLDR;** Any method which takes a *single* parameter can be used as an infix operator. Any method which does not require a parameter can be used as a postfix operator.

**example 1:** `a.+(b)` can also be written as `a + b`

**example 2:** `a.!` can also be written as `a!`

More Reading
- https://www.scala-exercises.org/std_lib/infix_prefix_and_postfix_operators
- http://www.cs.man.ac.uk/~pjj/cs212/fix.html
- http://www.codecommit.com/blog/scala/quick-explanation-of-scalas-syntax

## Nothing, Nil, Null, null, None
- https://oldfashionedsoftware.com/2008/08/20/a-post-about-nothing/

## Pattern Matching & Functional Composition
- Case Statements
- Currying
- Partially Applied Functions
- Partial Functions
- More Reading
    - [Stack Overflow: Currying vs Partially Applied Functions](https://stackoverflow.com/questions/14309501/scala-currying-vs-partially-applied-functions)
    - [Stack Overflow: Partial Functions](https://stackoverflow.com/questions/8650549/using-partial-functions-in-scala-how-does-it-work/8650639#8650639)

## Placeholder Syntax

## Handling Recursion

### Tail Recursion & Head Recursion

**TLDR;** In order to avoid stack overflow, use tail recursive methods optimized with the [@tailrec](http://www.scala-lang.org/api/2.12.0/scala/annotation/tailrec.html) annotation.


#### Background
On the JVM:
>
...if each call allocates a stack frame, then too much recursion will overflow the stack. Most functional programming languages solve this problem by eliminating stack frames through a process called tail-call optimisation. Unfortunately for Scala programmers, the JVM doesn't perform this optimisation....

>Because of these limitations, you need to be careful when using recursion in Scala. When writing programs, you will need to keep in mind how both the compiler and the JVM work. One safe approach is to use code from the standard library, where possible. For example, you'll find that many recursive algorithms can easily be rewritten in terms of standard operations like map and fold.

On the Difference between Tail & Head Recursion
> There are two basic kinds of recursion: head recursion and tail recursion.  In head recursion, a function makes its recursive call and then performs some more calculations, maybe using the result of the recursive call, for example.  In a tail recursive function, all calculations happen first and the recursive call is the last thing that happens.

via  https://oldfashionedsoftware.com/2008/09/27/tail-recursion-basics-in-scala/

##### Example
>If you do find a call that you think should be optimised by the compiler, but isn't, then you should check that the call:
is a tail call,
is in a final method or local function, and
is to itself.
For example, the code for factorial below would not be optimised. The call is not in tail position (the tail operation is the multiplication), and the method is public and non-final, so it could be overridden by a subclass.

```
class Factorial1 {
  def factorial(n: Int): Int = {
    if (n <= 1) 1
    else n * factorial(n - 1)
  }
}
```

> You can make simple changes to factorial to eliminate both of these problems. First, you could move the recursive code into a local function within the method, so that it cannot be overridden. Second, you could introduce an accumulator so that multiplication happens before the recursive call. Finally, you could add a @tailrec annotation so that you can be sure that your changes have worked.

```
import scala.annotation.tailrec

class Factorial2 {
  def factorial(n: Int): Int = {
    @tailrec def factorialAcc(acc: Int, n: Int): Int = {
      if (n <= 1) acc
      else factorialAcc(n * acc, n - 1)
    }
    factorialAcc(1, n)
  }
}
```

via http://blog.richdougherty.com/2009/04/tail-calls-tailrec-and-trampolines.html

### Thunks & Trampolines

**TLDR;** When you can't recurse, trampoline!

[Trampoline Example](http://blog.richdougherty.com/2009/04/tail-calls-tailrec-and-trampolines.html)

#### Definition
> A trampoline is a loop that iteratively invokes thunk-returning functions (continuation-passing style). A single trampoline suffices to express all control transfers of a program; a program so expressed is trampolined, or in trampolined style; converting a program to trampolined style is trampolining. Programmers can use trampolined functions to implement tail-recursive function calls in stack-oriented programming languages.

via  [Wikipedia](https://en.wikipedia.org/wiki/Trampoline_(computing)

## Type Inference

## REPL
- Getting started
- Fun Fact
- Warnings
    - type `:warnings` to see the deprecation message
    - via http://alvinalexander.com/scala/scala-repl-there-were-deprecation-warnings-re-run

## Other:

- [Testing](http://www.scalatest.org/user_guide) `Libraries`
- [Twitter Effective Scala](http://twitter.github.io/effectivescala/) `Style`
- [Twitter Scala School](https://twitter.github.io/scala_school/)
- [3 Tips for Writing Performant Scala](https://www.sumologic.com/blog/technology/3-tips-for-writing-performant-scala/)
