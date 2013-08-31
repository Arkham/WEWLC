# The Seam Model

One of the main things that everyone notices when they try to write tests for
existing code is how it is so poorly suited for testing.

## A huge sheet of text

With procedural code, software is just a _huge sheet of text_, a listing of
instructions, that we have to understand line by line.

## Seams

When you start to pull out individual classes for unit testing, you have to
break a lot of dependencies. Generally, we also want to avoid performing
expensive operations when executing our test suite. Here the concept of a _seam_
helps us:

> A seam is a place where you can alter the behaviour of the code without
editing it in place

Why are seams good? If we can change behaviour without changing the code, we
can therefore exclude dependencies without changing the code. With less deps,
we are able to write better unit tests.

## Seam Types

### Preprocessing seams

In C and C++, a macro preprocessor runs before the compiler: this can be exploited
to change code behaviour before the compiler compiles the code. Therefore, we
could use `#ifdef` and `#define` to change our code before compiling it.

It is very important to understand that each seam has an _enabling point_, a
place where you can decide which behaviour to use. In this case the `#ifdef` is
the seam enabling point.

### Link seams

In some languages, the compilation isn't the last step of the build process,
but there is an additional step, the linking. In this step, calls to external
code are resolved so that we have a complete representation of the program.

In this case the seams are the calls to external methods and the enabling points
are either classpaths or environment configurations.

## Object seams

These are the most common seams we can see in OOP languages. What is important
is that we need to have an enabling point where we can either dinamically inject
a fake object or have a method we can override in our test. In this way we don't
need to modify too much of our production code in order to setup some tests.

