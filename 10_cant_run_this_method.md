# Can't run this method in a test harness

The most usual problems that we may run into when executing methods are:

* the method is not accessible to the class (i.e. private)
* it's hard to call the method because it's hard to construct the parameters
* the method has very bad side effects
* we have to sense through other objects that the method uses, aka we have to
  check inputs and outputs of other objects

## The case of hidden method

If we ever need to test a private method, we should make it public. If this
bothers us, it could be because:

* the client won't ever need this public method
* if the client calls this public method, something bad can happen

In this cases, it would be better to move the behaviour to a different class as
public methods and then create a private instance of that in our class.

**Good design is testable, and design which isn't testable is bad**

Another trick we could use is to make the method protected and call it from a
fake subclass.

## The case of the helpful language feature

In some languages, some classes can't be subclassed nor instantiated (final in
Java or sealed in C#). In those cases, what we can do is to wrap the
unmodifiable objects inside a wrapper and use that wrapper as a seam to insert
our testing behaviour.

## The case of the undetectable side effect

Often we call methods which do some work, but we never get to know about it. Our
object calls methods on other objects, but we haven't a clue on how things
turned out.

*Command/Query separation*

A method should be a command o a query, but never both. A command is a method
which can modify the state of the object but that doesn't return a value. A
query is a method which returns a value but does not modify the object.

Why is this important? First of all, it is a code communication issue: if a
method is a query, we don't have to actually read its implementation to know if
we can use it several times in a row without causing some bad side effects.

In general, the trick here is to isolate the command into a separate method,
then **Subclass and Override method** to replace the call to the external object
with a fake stub implementation.

