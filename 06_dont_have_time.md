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

## Wrap method

1. When you first create a method, it does one thing.
2. When you add code, you probably want it to execute at the same time of the
   original method
3. This is called **temporal coupling**, and it is pretty nasty: you are
   coupling things only because they happen at the same time

Example:

```ruby
class Employee
  def pay
    account += week_hours.inject(:+) * rate
  end
end
```

Let's say that when we pay the employee, we want to log this action to a file;
we could add the code directly to the pay method or we could use the **Wrap
Method** tecnique:

```ruby
class Employee
  def pay
    log_payment
    perform_payment
  end

  private

  def log_payment
    log << "#{name} payed"
  end

  def perform_payment
    account += week_hours.inject(:+) * rate
  end
end
```

In this way, we've added some behaviour to the pay method, but we have kept our
changes more clean and the clients can keep using the same interface. If we
don't want the logging to be performed every time, we could use the **Wrap
Method** tecnique in another way:

```ruby
class Employee
  def pay
    perform_payment
  end

  def logged_pay
    log_payment
    perform_payment
  end

  private

  def log_payment
    log << "#{name} payed"
  end

  def perform_payment
    account += week_hours.inject(:+) * rate
  end
end
```

In this way, both ways of payment are supported. The main problem with this
tecnique is coming up with new names for the old methods: `perform_payment`
doesn't sound too good. In fact, we could refactor this to make it more
understandable:

```ruby
class Employee
  def pay
    amount = calculate_payment
    log_payment(amount)
    perform_payment(amount)
  end

  private

  def log_payment(amount)
    log << "#{name} payed #{amount}"
  end

  def perform_payment(amount)
    account += amount
  end

  def calculate_payment
    week_hours.inject(:+) * rate
  end
end
```

In order to use this tecnique you should:

1. Identify the method you want to change
2. If the change can be written as a single sequence of statements in one place,
   rename the method and then create a new method with the same name and
   signature of the old method.
3. Place a call to the old method in the new method
4. Develop a new method in TDD and call it in the new method

## Wrap Class

This is the class level alternative to the Wrap Method tecnique. Using the same
example from the previos section, we would create a new class which wraps the
old functionality:

```ruby
class LoggingEmployee
  attr_reader :employee

  def initialize(employee)
    @employee = employee
  end

  def pay
    log_payment
    employee.pay
  end

  private

  def log_payment
    log << "#{employee.name} payed"
  end
end
```

This tecnique is called the *decorator pattern*: basically, we create new
objects which wrap the behaviour of existing objects and pass them around. The
new objects should have the same interface of the old ones so that clients
shouldn't realize that they are working with a wrapper.

Over time, you should pay attention to the responsibilities of the wrapper and
see if it can become another high level concept of your system.

The difference between using the **Sprout Method** or the **Wrap Method**
tecnique depend if you want to add a piece of logic to an existing method, or if
the new logic is as important as the old one.

The **Wrap Class** tecnique is useful when:

* the behaviour i want to add is completely independent and I don't want to mix
  this in the existing class
* the existing class has grown so much that I can't stand it to make it worse.
  In this case, I wrap only to make a note-to-self for later changes.

**The biggest ostacle to improvement in large codebases is the existing code**

The ugly code makes every good attempt look useless. The problem is that, piece
by piece, the code gets uglier; if we instead start with little improvements the
code will start to look much better after a couple of months.

