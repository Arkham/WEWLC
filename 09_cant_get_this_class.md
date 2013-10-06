# Can't get this class in a test harness

The problem you generally encounter when setting up a test harness are:

1. Objects of a class can't be created easily
2. The test harness won't easily build with the class in it
3. The constructor we need has bad side effects
4. Significant work happens in the constructor, and we have to stub it

## The case of irritating parameter

When we need to create an object in a test harness, the best approach is to just
try to do it: write a test case and attemp to create an object in it. This kind
of test is a **Construction Test**.

When we try to build this kind of test, we may discover that the object needs
some dependencies in order to be created: some of these dependencies may be easy
to build, but some of them are not.

These are the **irritating parameters** of our test: they may be connecting to
external servers or requiring specific hardware in order to work correctly. What
we can do to make our life easier is to replace them with fake objects.

The problem with these fake objects is that have little or no relatioship with
the actual production code: they generally return constants or fake values just
to make testing possible. That's the point: the same rules do not apply for
production code and code which makes testing possible.

If it is very hard to construct a parameter, we may also consider to pass
**Null** instead: in this way, if we aren't using the object in our tests,
nothing will happen; if instead the test uses the parameter, the testing harness
will tell us exactly where and when.

Note that we should never pass null in production systems: chances are that our
code will be full of nil-checks and be very brittle in the end. Instead we might
want to use the following pattern.

### Null Object Pattern

This tecnique helps us stop abusing null in our applications: instead of
returning null if a employee is not found, we return a particular employee, an
instance of NullEmployee, which has empty name and email. This kind of object
prevents clients from having to perform null checking.

## The Case of the Hidden Dependency

Some classes are deceptive, they look easy to instantiate, so we try to put it
inside a test and boom, we run into an obstacle: the object uses a resource we
can't access inside our test harness.

This problem is easy to fix with **Parametrize Constructor**: we have to
refactor the method so that the hidden dependency is explicitly passed, so that
we can replace it with a fake object in our tests. Moreover, we can setup somo
commodity methods so that the change remains transparent for our clients, which
can keep using the old interface.

## The Case of the Construction Blob

*Parametrize Constructor* is a good tecnique, but it the object we are working
with is creating too many objects, it may be hard to use it; instead, we may
use **Supersede Instance Variable** and replace the dependency manually, so that
the test uses the fake object. Although, we have to be extra careful that this method
isn't used in production code.

## The Case of the Irritating Global Dependency

One of the hardest problems to solve when we want to create and use classes in
tests is the widespread use of global variables. Unfortunately, in these cases
it is very hard to use *Parametrize Method* because the use of globals is so
common that it would require a considerable effort to do so.

Commonly, in OOP codebases global variables are defined as singleton objects:
the problem with these objects is that their design states that there should be
only a single instance of them across the whole system.

Instead, we want to make new instances for better test isolation: what we can do
is to add a setter to the singleton class in order to inject a new instance or
reset the existing one within our tests.

We need to understand why singletons are used:

* we are modeling the real world, and there is only one of these things
* if two of these objects are created, bad things happen
* if someone creates two of these objects, we'll use too many resources

In these cases, if we can't avoid using the singleton objects, we can subclass
and override the singleton methods, so that it doesn't perform actions such as
talking to the database or connecting to external services.

## The Case of Horrible Include Dependencies

The problem in C++ is that a file may include a file which includes another
file: the result is that when trying to test a class, we have to solve the
problem of header dependency.

Instead of including everything the original files include, we may include the
dependencies one at a time and use alternate implementations of the libraries
methods in order to keep our tests fast. Unfortunately, in this way we are
duplicating the definitions of those methods, so we will need to keep these
declarations in sync.

## The Case of the Onion Parameter

Objects inside of other objects: they seem like a big onion. The biggest problem
they cause is that in order to put them inside a test, we need to instantiate
their dependencies, which need their own dependencies instantiated aswell.

The best way to handle this is to break the dependency chain as soon as possible
and replace the dependency with a fake object. In any language where we can
create interfaces or classes which can act as interfaces, we can systematically
use them to break dependencies.

## The Case of the Aliased Parameter

Sometimes, when we are using *Extract Interface* to isolate the objects in our
tests, we may notice that we are duplicating the objects hierarchy from our
production code in our testing code. Interfaces are great at breaking
dependencies, but when they get to the point that there is a one-to-one
relationship between classes and interfaces, something is wrong.

Instead of completely severing the relationships between the two objects, we
could try to use *Subclass and Override Method* just to isolate enough of the
original method so that our tests don't get too slow.
