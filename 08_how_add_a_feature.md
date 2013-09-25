# How Do I Add a Feature?

In legacy code, we don't have tests around the code. Getting them in place may
be very difficult.

Tecniques like sprout or wrap may work, but we don' significantly modify the
existing code, so it's not going to get better.

So we need to have a solid foundation from where to start with. Once that is
done, we can start adding new features.

## Test Driven Development

One of the most powerful tecnique for adding features is TDD. It is basically
like this:

1. Imagine a method that will help us solve the problem
2. Write a failing test for it
3. Write any code that make the test pass
4. Refactor to remove duplication
5. Repeat

TDD helps us concentrating on one thing at a time. We are either:

* writing new code
* refactoring old code

But we never do both at the same time. In legacy code, this is particularly
important because it separates the act of writing new code from the act of
refactoring old code.

## Programming by Difference

In OOP, using inheritance can solve problems very quickly: whenever we have a
slightly different behaviour in our system, we can create a subclass which
ovverrides the original behaviour and implements the desired behaviour.

What's the problem with this tecnique? If we don't pay attention, the quality of
our code degrades rapidly. What happens when we need *two* slightly different
behaviours? The problem with inheritance is that the distinct features are in
different subclasses..

If we added tests to our subclasses we can refactor and be sure to keep the
original behaviour: we can substitute the inheritance with options or with
object composition.

> There are many powerful refactoring, but Rename Class is the most powerful. It
> changes the way people see code and lets them notice possibilities that they
> might not have considered before.

**Liskov substitution principle**

> Objects of subclasses should be substitutable for objects of their superclass
> throughout the code; if they aren't, we may have silent bugs in our code.

*Square < Rectangle example*

Unfortunately, there aren't sure ways to avoid LSP violations. Some steps we
could follow are:

1. Whenever possible, avoid overriding concrete methods
2. If you do, see if you can call the method you are substituting in the new
   overidding method

*Programming by Difference* lets us introduce variations quickly in our system;
when we use it, we use tests to pin down the differences, so that later we can
refactor them easily.
