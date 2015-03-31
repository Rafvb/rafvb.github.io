---
layout: post
title: "TDD: How to keep it from going wrong (Part 3: When you're using it wrong)"
published: true
---

## Preface
Over the years TDD has been widely adopted by the programming community. TDD can be wonderful when done right, but can harm your code and design
when done incorrect. These series tries to pinpoint some of the pitfalls and how to circumvent them.

See the [intro post](/TDDHowToKeepItFromGoingWrong1/) for more details and references.

## How I used to TDD
In [the previous post](/TDDHowToKeepItFromGoingWrong1/) we saw the concept of TDD. It is fiendishly simple, but when you put it in to practice you might bump into some questions or pitfalls.

When I started with TDD, I made a mistake that a lot of people make when first doing TDD. I misunderstood the concept *unit* test. Also most of the guides that I read at that time did not help. 
They guided me down the wrong path.

The way I understood the unit concept of a unit test was that the code you are testing should be a single unit. This of course maps nicely to the OO concept of classes.
So a test should only test a single unit/class in isolation. Everything else should be mocked/stubbed out of the way. Using this technique we could focus nicely on the behaviour of that single class, not having 
to worry about the implementations of it's dependencies. As an added bonus, we thought, we where creating a really nice decoupled design.

We where wrong.

In the beginning all of this worked out well. Sure, it was a bit of extra work to create those interfaces needed to mock the dependencies. But we where decoupling, right!
But when the time came to add functionality, fix a bug or refactor our code, we had problems. We needed to move a lot of tests, because behaviour moved between classes. We also needed to change a lot of tests for what 
seemed like a small code change. In the end, it felt like the time and effort we where putting in simple keeping the tests running versus the time we spent changing the production code was out of balance.

## An example
Lets look at a simple example. This is common example we also use to illustrate a code smell in our Agile Development trainings at Cegeka.

<script src="https://gist.github.com/Rafvb/8daf608346838e6e0e4d.js"></script>

Lets say this piece of code resides in a PaperBoy class. This method is used when a paper was delivered and the customer has to pay up.
This code has a problem. It smells. The code smell we see here is feature envy. The PaperBoy is way to intimate with the customers wallet.
Image a paper being delivered to your house, and the paperboy grabbing your wallet and taking the money from it ("but I would like to use my credit card?").

So you should refactor the code. We want the customer to be able to make the payment when asked by the paperboy. In what manner the customer makes this payment is 
of no interest to the paperboy, as long as it is done.

## Completely loosely coupled
Now if you've TDD to the letter, as we believed we where doing in the beginning you end up with something like this:

ICustomer customer = ...
IWallet wallet = customer.Wallet;

If we tear open the first example, we can see that we have created some nice interfaces for our customer and our wallet.
In our tests we stubbed these interfaces, so our paperboy is complete decoupled from the implementations of customer and wallet.
We where able to complete write the paperboy class against these interfaces, without the need for an implementation of customer of wallet. Sounds great?
Nope. We've strayed off the nice TDD path, into the wilderness.

## Trying to refactor
So we want to refactor. We want to move the payment logic, from our paperboy into our customer. In golden TDD land, we should be able to do this, without our tests breaking and 
we would know we had not made a mistake.

We would like to get to something like this:

public void ReceivePayment(decimal amountToPay)
{
	customer.payUp(amountToPay);
}

But if we do this, our PaperBoy tests will break. We will have to stub a different method (payUp). And we will have to move some of the tests over to the customer test class.
Out tests are tightly coupled to our implementation. It feels painful to refactor. In the end, you waste more time on making tests work again. In the long run, you could get an 
aversion of refactoring or worse, delete your tests.

## Whats wrong with the code
As pointed out, our tests are too tightly coupled to our implementation. We know the code is calling the TotalMoney property and using the subtractMoney method of the wallet.

Another really big problem is the fact that we are creating interfaces for all our domain classes.

Why not create an interface. It costs almost nothing, and we can plug in a different class if we want too.
Two things. Almost no cost, is still a cost. You are going to have to maintain that interface in the long run.
Secondly, as we saw on our project, I could count the places where we plugged in a different implementation for the same interface on a single hand.
But yet we had hundreds of interfaces with only one implementation.

The fact that we create an ICustomer and a Customer implementation should ring an alarm bell. First of all, the fact that we cant name them better than this points to a useless interface.
Secondly, we probably only have a single notion of a Customer in our domain model. Why then the abstraction? (If you do have multiple types of customers in your domain model, than this of course is a valid case).

On top of that we have the problem that, because we are using an interface, we have to make all the methods on it public.
So the subtractMoney method has to be public. Maybe you don't want anyone outside your domain model to just take money from the customers wallet. But it's in the interface, so it has to be public.

And a final point: how much are the tests worth anyway? We are using stubs for the customer and the wallet. That means we fake their behaviour in the test class of PaperBoy.
So if we for example have a test that asks for a negative amount from the subtractMoney method. The current behaviour of a wallet ignores negative values, so we stub it to do nothing in our paperboy test class.
At a later stage it is decided that the wallet should throw an exception for negative values. If we forget to update our stub, the test will still run, but a user could get unexpected behaviour at runtime.
Loose coupling is nice, but this is a bit too much.

## How can we fix this?
You probably get where I'm going with this? :)

In the example above, we don't want an interface for Customer, or Wallet. Our tests should use the real implementations. We move away from the thinking that we need to have a test class per production class.
We write functional unit tests, still asserting one tiny piece, but whose implementation is allowed to span multiple classes. We test the public interfaces of the classes, not caring what they do internally.

So in the above example, if we did not use interfaces, we would have been able to just move the method from the paperboy to the customer and our tests would still run, provided the end result was still the same: the money was substracted.

We'll dive some more into this in the next blog post, because of course this isn't as black/white as this simple example. Interfaces aren't evil, they have valid uses, but like everything, with moderation.
So don't go and throw out all your interfaces just yet ;)

## What about legacy code?
A small word on legacy code. When your working with old, dirty code, all bets are off. You are allowed to break open code, make stuff public that shouldn't be public, add interfaces if it helps to get the code under test.
If you keep on doing this, the code will get cleaner over time. So all of the 'do nots' we discuss here can be allowed when working with legacy code.

An excellent book, if you want to read more on the subject, is [Working effectively with legacy code](http://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052).

## Posts in this series
* [Part 1: Introduction](/TDDHowToKeepItFromGoingWrong1/)
* [Part 2: What is TDD and why do we it](/TDDHowToKeepItFromGoingWrong2/)
* Part 3: When you're using it wrong
* More to appear soon...