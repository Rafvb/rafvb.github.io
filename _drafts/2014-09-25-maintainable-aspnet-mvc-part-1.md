---
published: false
---

## Maintainable ASP.NET MVC Part 1: Setting up coded UI test on ASP.NET MVC using Selenium

I'm doing a series of posts on developing a maintainable ASP.NET MVC website. In this first post we'll be looking at setting up a basic website and using Selenium to create coded UI tests. These tests will be the driver for the implementation we'll be creating.

### The case
The website we'll be creating will let us manage our monthly spendings. The user will be able to add expenses and get an overview of them per month.

A simple case, but enough to demonstrate the set-up we'll create.

## Creating the project
Fire up your Visual Studio (I'm using VS2013 R3) and create a new project. Select ASP.NET Web Application and name the solution **BudgetMeter** and the project **BudgetMeter.Web**.

[CreateNewProject.PNG]

In the next screen, select the MVC template and also check the core references for Web API. We won't use any authentication, so click Change Authentication and select No Authentication.

[NewASPNETProject.PNG]

We have now created a neat little playground for us to mess up :)

## Add the coded ui tests project
Now we will create our first coded ui test, but we will seperate them into a seperate project. Add a new Unit Test Project to the solution and name it **BudgetMeter.CodedUITests**.

[CreateNewCodedUITestsProject.PNG]

_Why no Coded UI Test project type?_

_We will not be using the Coded UI tools provided by Visual Studio. Using Selenium directly is easy (in my opninion even easier than the coded ui tools). Plus if we structure them correctly, we can get much cleaner and maintainable testing code._

Next we'll add Selenium into the test project.

## Adding Selenium
Richt click on the coded ui project and select _Manage NuGet Pacakges_. Search for _Selenium WebDriver_. Install the _Selenium WebDriver_ (contains the actual web driver) and _Selenium WebDriver Support Classes_ (helps us select elements on the page, wait for things to load, ...) packages.

To start off we will do our tests in Internet Explorer. Chrome and Firefox work out of the box, and in the next posts I'll show you how you can also go 'headless', but IE requires an extra step to work. We'll get to that step in a moment.

Let's write our first test now!



### Resources
A lot of inspiriration was found:

http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627