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

This problem is easy to fix: we have to refactor the method so that the hidden
dependency is explicitly passed, so that we can replace it with a fake object in
our tests. Moreover, we can setup somo commodity methods so that the change
remains transparent for our clients, which can keep using the old interface.



