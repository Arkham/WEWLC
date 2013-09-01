# Tools

## Automated Refactoring Tools

What is refactoring?

> Refactoring is a change made to the internal structure of the software to
> make it easier to understand and cheaper to modify without changing its
> existing behaviour.

When using automated refactoring tools, we have to be sure that these tools do
not change the behaviour of our code; we should have tests which cover the whole
behaviour and assure us that the refactoring didn't break stuff.

## Mock Objects

In order to break dependencies in legacy code, we have to replace actual objects
with fake ones which supply the right values when we are testing: in this way,
we can be sure that the code is being executed thoroughly.

## Unit Testing Harnesses

The most effective testing tools are generally free. The first one that comes in
mind in xUnit, a small and powerful unit-testing framework developed originally
in Smalltalk by Kent Beck. Its main features are:

* Write tests in the same language people are developing with
* All tests run in isolation
* Tests can be grouped in suites so that they can be rerun on demand

xUnit family is composed by many frameworks, notably JUnit for Java, CppUnit and
CppUnit for C++ or TestUnit for Ruby.

### Common features

* each test should create a separate object to test, in order to maintain isolation
* there can be a setup method to be run before each test..
* and a teardown method to be run after each test

## General Test Harnesses

While xUnit style tests are more designed for unit testing, there are other
frameworks which are more focused on testing multiple classes at once, which are
FIT and Fitnesse.

### Framework for Integrated Tests (FIT)

FIT is a framework developed by Ward Cunningham. Using FIT you can describe the
behaviour of your system and write inputs and outputs as HTML tables and FIT
will run these tables as tests.

FIT accepts HTML, runs tests defined in HTML tables and produces HTML output,
displaying in green passed tests and in red failed tests.

The powerful concept behind FIT is that it helps communication between the
developers who write the software and the clients who need to specify what it
should do. In theory, clients could write new tests (which won't pass) and the
job of the developer will be to make those pass.

### Fitnesse

Fitnesse is basically FIT inside a wiki and was built by Robert Martin.
