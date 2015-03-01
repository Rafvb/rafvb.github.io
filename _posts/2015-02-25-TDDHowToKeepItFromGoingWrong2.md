---
layout: post
title: "TDD: How to keep it from going wrong (Part 2: What is TDD and why do we it)"
published: true
---

## Preface
Over the years TDD has been widely adopted by the programming community. TDD can be wonderful when done right, but can harm your code and design
when done incorrect. These series tries to pinpoint some of the pitfalls and how to circumvent them.

See the [intro post](/TDDHowToKeepItFromGoingWrong1/) for more details and references.

## What is TDD
I'm assuming you already know TDD and have applied it on your projects. But to give a small recap.
Test Driven Development is a way of developing in which we write our tests first and let our code grow from there. It consists of 3 seemingly simple steps:

* Write a failing tests
* Make it pass
* Remove duplication (refactor)

It's these easy steps that cause so much confusion and misinterpretation.

Lets look at the steps in more detail.

### Write a failing tests
In this step, you think about your code from the outside, from the perspective of the user.
You write a test, with the minimal amount of code you can and you make sure it fails.

Failure is important here!

You could have written a test that fails for a reason that you did not expect or a test that passes
without you expecting it to pass. Maybe your test is wrong (for example a problem in the setup) or maybe
it shows the code did something you did not expect.

This step is the test for your test. Make sure it fails and, equally important, make sure it shows a readable failure message.
You should be able to interpret the result of the failure in a glance. When this tests starts to fail in a month, or a year, or longer, you
want it to be as clear as possible.

What you should have now is a failing test, and the minimal amount of production code needed to get the test to compile (so for example a class with one empty method).

### Make it pass
In this step, you write the production code that will make your test pass.
If the code is really obvious to write, just write it.
If you have no idea how to implement the production code, fake it. This means for instance returning a hard coded value.

In this step, you are still more concerned about your test than your production code. You want test the green state of your test.
This is something I missed when first doing TDD. I would just write my clean production code and get onto my next test.

What you want to do is make it as simple as possible for yourself (lazy programmers, we are :) ). Focus on one thing at a time.
In this step you can commit any design sin you want to get your test to pass. If the production code you write here makes you want to cry, no problem.
We'll get to that in the next step. Just make sure you have a green test after this step.

### Remove duplication (refactor)
And now we get to the best part of TDD. This is the step most of the beginners have difficulties with (I know I did).

In this step, you clean up that steaming pile of production code you just wrote and turn it into the most beautiful design the world has ever known.
The code will make you cry, but this time because it's so pretty :)

The main driver behind the clean-up should be the removal of duplication. If you have a hard coded value in your production code from the previous step, that's
probably a direct duplication from a value in your test, so you should remove that (make it into a parameter for example).

This is also the step where you can let loose all your refactoring skills on a crusade to rid your code of the evil code smells.
You do a small refactoring and run your tests to see if you didn't break anything. If you did, you roll back the small step, if not you move on to the next step.

Now, if done correctly, this step means that you are moving code around, making it more readable, breaking out methods, classes, ... all without having to modify
your tests. This can really be a glorious feeling, but this can also be the pace where you feel the pain of TDD. If you do TDD incorrectly, you don't get the 
safe refactoring benefits and end up with tests that are tightly coupled to your implementation, meaning that if you change some code, a test fails or even doesn't compile any more.
This is what well be addressing in the remainder of these posts.

So after you have cleaned up your code, the cycle restarts and you write a failing test again. When you get into this flow, it really is a matter of minutes for a full cycle to complete.

## Why do we TDD
So what benefits do we get out of TDD?

* Clean code
* Executable documentation
* A test harness

We get clean, readable code, because we write only the production code we need to get a test pass. I'm not a fan of the code coverage metric, but code written using the TDD cycle
should have a coverage of 100%. Also, because we refactor after every test, our code will always be in a clean state.

Because we have simple tests, that exercise a single use case in our code, our tests are executable documentation. Other programmers can see the way our classes are used in the tests and see
the outcomes. Because they execute the code, they are always up-to-date with the code.

Lastly, one of the most important benefits it the test harness around our production code. We should get a sense of safety when making changes to our code, because our tests create a safety net
around it. We should be able to move around and change code freely (without changing the functionality) and feel confidant that if no test fails, we have done no harm to our code.

Of course, no method can prevent bugs from appearing, but the feeling of confidence in his/her code is still what a programmer should strive for, and what TDD can help with.

In the next post, we'll see some of the problems that can prevent you from reaching this happy place, where you have TDD'ed your way into confidence in your code.
We'll see some code samples on when TDD can feel wrong, and what we can do about it.

## Reference
If you want to learn more about TDD, the first place you should go is the fantastic book by Kent Beck, [TDD by example](http://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530).

## Posts in this series
* [Part 1: Introduction](/TDDHowToKeepItFromGoingWrong1/)
* Part 2: What is TDD and why do we it
* More to appear soon...