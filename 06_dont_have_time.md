# I Don't Have Much Time and I Have to Change It

Is breaking dependencies and writing tests worth it? *We don't know*

We don't know because once we do it, we can't possibly can't possibly know the
time we might taking fixing bugs, adding new features and the time we would
normally spend to make sure it is working.

In the end, testing makes development faster because it makes it more
*confident*.  It makes work much less frustrating.

The problem with doing something quick and dirty is that you generally tell
yourself "I'm going back and test it and refactor it that when I have time".
The main problem is that moment generally never comes.

So the real question is: pay now or pay later?

## Sprout method

When you need to add a new feature and it can be formulated as new code, write
it in a new method and call it from where the functionality needs to be. You may
not be able to test those points, but you can test the newly added code.

The general idea is to change the old code as _less_ as possible: we need to
focus on extracting the new feature in a completed new method and inject it in
other places.

The basic steps to follow are:

1. Identify where you need to make the change to the code
2. If the change can be formulated as a single sequence of statements, write a
   call to a new method that will do the work involved and comment it out
   temporarily.
3. Understand which local variables you need to pass to the new method and make
   them arguments to the call
4. Determine if the new method will need to return values to the source method
   and assign these values correctly
5. Develop the sprout method using TDD
6. Remove the comments in the source method to enable the call.

The sprout method is so much better that adding the code inline. Although, it is
basically saying "I'm giving up on the source method, but at least this new
piece of code will be tested".

## Sprout class

Sometimes we can't use the sprout method tecnique because the source class may
be very hard to get inside a test harness or there is no simple way to
instantiate all the objects you need to setup a test.

In these cases, it may be preferrable to create a totally new class to hold all
the changes and use it from the source class: as we do so, we may start noticing
patterns to extract and refactor code along the way.

In the end, we may also decide to fold back the sprouted class into the original
class: some other times, we may notice that the sprouted class is a new
important piece of our design.

The steps for creating a Sprout class are very similar to the Sprout method
ones; the main difference is to understand if a new feature is a responsibility
of the current class or it has the importance of being written in a new one
