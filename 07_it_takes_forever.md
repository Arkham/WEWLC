# It Takes Forever to Make a Change

How much time does it take to make a change? **It depends**

## Understanding

Whenever we add code to a system, we are adding complexity. Therefore, it is
unavoidable to make code more complex when a project grows; although, in a
well-maintained system it might take some time to figure out where to make the
change, but when you do, it feels easy and good to do it. In a legacy project,
it will take you some time to find where to change the code and it will be very
difficult to do it.

## Lag time

*Lag time* is the time period between the moment you make a change to the code
to the moment you get feedback for that change.

You should be able to get very fast feedback from the tests on the code you are
working on, and that feedback loop should not exceed 5-10 seconds.

## Breaking dependencies

The first and biggest obstacle in legacy code is to get a class inside a testing
harness: this can become a very large effort in some systems.

## Build dependencies

If we need to compile our code, we should organize our code into libraries which
are linked together: in this way, the cold-start total build time of our project
will grow, but the average build time will get faster. The trick is to
substitute the dependencies of our objects with new interfaces, which can be
represented by fake objects.

This tecnique gives us the freedom of not having to recompile the whole program
when one of its depencies are being changed.

**Dependency Inversion Principle**

> When your code depends on an interface, that dependency is generally very
> minor and unobstrusive. For this reason, it much better to depend on
> interfaces or abstract classes, than it is to depend on concrete classes.

There is some conceptual overhead in having more interfaces and packages. Is
that a fair price compared to the alternative? **Yes**. It might take some time
to find where to actually perform the change, but when you do, you can work with
them very easily.

## Summary

*Agile Software Development: Principles, Patterns and Practises, Robert Martin*
