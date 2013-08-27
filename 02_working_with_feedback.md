# Working with feedback

There are basically two ways to work with code:

* Edit and pray
* Cover and modify

## Edit and pray

This is the industry standard, and consists in the following steps:

1. Carefully plan the changes
2. Understand the code you are going to modify
3. Start making changes
4. Poke around to see if everything is working (this step is **very** important)
5. Repeat 3 and 4 until done

## Cover and modify

This is a totally different approach, the main idea is that there is a *safety net*
that protects the developer when making changes, so that new code doesn't break old
code which was known to be working.

1. Write tests to describe behaviour
2. Write code
3. Run tests to see if everything is working

Tests aren't only useful for testing code correctness, but also for **detecting change**

## Integration testing vs Unit testing

* Integration testing is slow (if unlucky, a full suite takes an hour)
* Unit testing is very fast (if unlucky, a full suite takes a second)
* Unit testing makes refactoring easier and safer

### Unit testing

Unit tests are tests which describe the behaviour of individual software components in *isolation*

**Why is it important that unit tests run in isolation?**

Large tests have the following issues:

* Error localization: errors are harder to find
* Execution time: tests take much longer time
* Code coverage: it is more difficult to create a high-level test to cover our new feature

On the other hand, small tests:

* Run fast
* Help us locating problems

An unit test is not an unit test if it's not *FAST*. How fast are we talking about?

**An unit test is slow if it takes more than 1/10 of a second to run**

### High-level testing

High-level tests (integration tests) help us describe our system behaviour at a
higher level of detail, describing scenario and real-life interactions

### Test covering

*How do we start making changes in a legacy project?*

We setup tests to cover existing behaviour.

*But what happens if it is very hard to test?*

We break dependencies to make testing easier.

**Legacy Code Dilemma: before making changes we should have tests, but in order
to have tests we have to change code**

The trick here is to make these changes *very* conservatively, suspend our
sense of aesthetics and cleanly break dependencies in order to be able to setup
our tests. Once the tests are in place, we can move on to improve the code design.

### The Legacy Code Change Algorithm

1. Identify change points
2. Find test points
3. Break dependencies
4. Write tests
5. Make changes and refactor

