---
published: false
---

## Maintainable ASP.NET MVC Part 1: Setting up coded UI test on ASP.NET MVC using Selenium

I'm doing a series of posts on developing a maintainable ASP.NET MVC website. In this first post we'll be looking at setting up a basic website and using Selenium to create coded UI tests. These tests will be the driver for the implementation we'll be creating.

### The case
The website we'll be creating will let us manage our monthly spendings. The user will be able to add expenses and get an overview of them per month.

A simple case, but enough to demonstrate the set-up we'll create.

## Creating the project
Fire up your Visual Studio (I'm using VS2013 R3) and create a new project. Select ASP.NET Web Application and name the solution BudgetMeter and the project BudgetMeter.Web.



### Resources
A lot of inspiriration was found:

http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627