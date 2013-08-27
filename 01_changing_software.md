# Changing software

There could be four reasons to change software:

* Adding a feature (adding new behaviour)
* Fixing a bug (changing old behaviour)
* Improving the design (behaviour remains the same, design improves aka refactoring)
* Optimizing resource usage (behaviours unmodified, resource usage optimization)

In each of these cases, generally the code we are touching is a small part of
the actual code size. Therefore, we want to change some behaviour, some functionality,
but we want to preserve so much more.

**We have to figure out how to preserve existing behaviour**

Otherwise, we don't know how much of that existing behaviour is at risk when we make
our changes.

To mitigate risks, we can ask ourselves these questions:

* What changes do we have to make?
* How do know that these changes work?
* How do know that we haven't broken anything else?

Generally, we are very scared to touch "known-to-be-working" code

**If it ain't broken, don't fix it!**

So the answers seems to be: avoid change as much as possible.

The problem is that, with time, the quality and understability of our code worsens:
we are no more confident in editing it and we hesitate when we have to modify
"known-to-be-working" pieces of code.

If no one touches a piece of code, no one is able to understand it: in time, you
won't be able to touch it because you've become too rusty at it. Therefore, there
are many holy pieces of software :)
