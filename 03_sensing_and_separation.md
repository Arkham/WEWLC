# Sensing and Separation

When we want to get tests in place, there are two reasons to break dependencies:

* __Sensing__ we break dependencies to _sense_ when we don't know the effects caused by our code
* __Separation__ we break dependencies to _separate_ when we can't get a piece of code inside a test

## Fake Collaborators

The greatest problem one can face when dealing with legacy code is dependency.
We just want to execute a piece of code by itself, and we want to break
dependencies. Moreover, the pieces of code on which we depend are sometimes the
only places we can sense the effects of our actions.

We can write tests when we can substitute our dependencies with something else.

### Fake Objects

A _fake object_ impersonates a collaborator of the class which is being tested.

A good way for inserting a fake object is to extract an interface and move the
current logic inside a new class; in this way, we can dinamically inject a fake
object and test that it behaves as we think it does.

### The Two Sides of a Fake Object

The fake object have to adhere to the extracted interface, but it will probably
have more/less methods and variables than the original object. Therefore, it looks
like the original object, but it's not 100% adherent.

### Mock objects

Mock objects are fakes which perform assertions internally: therefore, we can
tell a mock what to expect and verify that these expectations are met.
