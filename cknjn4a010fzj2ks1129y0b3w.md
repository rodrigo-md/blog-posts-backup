## What every developer should know to start writing tests

We test things daily, from _"how hot or cold the water is before entering the shower"_ to _"taste and qualify a new meal before buying it again."_

Sometimes we don't test things directly, but we adjust our behavior based on others' feedback, like _reading a product's review before buying it on the internet._

We know about **testing** and **feedback**, but apparently, some companies and developers believe that they can skip that process as long as they deliver a working product.

They can't be more wrong...

A product that works is just the tip of the iceberg. Once it solves someone else's problem, it needs to improve its quality and be easy to maintain to secure its future; and even better when the product is built from the start on quality.

That's why developers who know how to write tests are attractive to recruiters, companies, and future team members because they can ensure quality, saving time and money.

If you don't believe me, down is the face of a Tech Lead interviewing a candidate who says he **_knows how to write automated tests_** 👇👇👇


%[https://media.giphy.com/media/l4EpfbRWMHRGhUjRe/giphy.gif]

[comment]:# (# Table of contents)
[//]:# (* [What's testing?](#what-is-testing))
[//]:# (* [What happens when we don't test our code?](#what-happens-when-we-dont-test-our-code))
[//]:# (* [Why manual testing isn't enough?](#why-manual-testing-isnt-enough))
[//]:# (* [What is an automated test?](#what-is-an-automated-test))
[//]:# (* [Benefits of automating testing](#benefits-of-automated-testing))
[//]:# (* [Example: A simple automated test](#example-a-simple-automated-test))
[//]:# (* [How pros do it?](#how-pros-do-it))
[//]:# (* [How can we test?](#how-can-we-test))
 [//]:# (* [Choose wisely the type of test](#1-choose-wisely-the-type-of-test))
 [//]:# (* [Prepare the environment](#2-prepare-the-environment))
 [//]:# (* [Anatomy of a functional test](#3-anatomy-of-a-functional-test))
[//]:# (* [OK. They are great but, What about time? Will we go slower?](#ok-they-are-great-but-what-about-time-will-we-go-slower))
[//]:# (* [If are so great, why some devs avoid writing automated tests?](#if-are-so-great-why-some-devs-avoid-writing-automated-tests))
[//]:# (* [Conclusion](#conclusion))
[//]:# ( [What's next?](#whats-next))




# What is testing?

_Testing_, or more formally _software testing_, is performed to ensure the software behaves as expected by putting the production code under different scenarios, conditions, and situations. It also works to discover any problem that the team didn't consider during the software's conception.

Although testing is a broader discipline that involves different roles, such as QA testers and software developers, **in this article, we'll focus on what a developer can do to contribute to quality assurance.**

# What happens when we don't test our code?

The time to deliver software is shortened every day, meaning that we need to ship new features and reach competitors' delivery capabilities sooner; otherwise, we'll be quick-off out of the market. The situation left no space for errors. Therefore, companies prefer to invest in improving the product's quality before it gets to the user's hands.

The reasons can vary, but we could lose our customer trust because of having too many errors or be replaced by our competition by not shipping features faster.

# Why manual testing isn't enough?

![product increment vs iterations_with_logo_final.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1617854853357/VdopwS2rt.png)

Although manual testing can help to increase the quality of a product, it has three main problems:
* Is NOT scalable
* Is NOT fast
* Is NOT repeatable

These problems come into account, especially when our software begins to grow; more lines of code result in a growing density of defects which increases the complexity of testing the product manually.

If the number of defects surpasses the QA team's capabilities, they will pass from a **proactive attitude to a reactive one**. If that happens, they will stop every improvement and future ideas, prioritizing the defects.  The problem can also occur in teams where developers and QA testers collaborate in the same team.

The amount of people restricts manual testing. There are a limited number of tasks a human can take. If the number of defects is greater, eventually, those will pass to production and will have to be solved, cutting the amount of time to build new features and improve the code.

There are other problems caused by doing manual testing while developing :
* **Create over-complicated solutions, unintentional:** When we start immediately to work on something without considering how we'll test the code, we ended up with code that's hard to maintain.
*  **Requiring extra documentation to understand what to test:** Most likely, only two people will know how to test the code, the person who wrote it and the QA tester, if he exists. To remove the authorship of code, the team will need to document the testing process so that anyone could test it in the future.
* **Fear of refactoring:** By not having a test to rely on, and execute whenever we can, developers will avoid improving, separating, and changing extensive code because of the fear of breaking something without knowing.

All of us do manual testing at a certain point. It's valuable to get visual insight and to discover flaws in the system by taking unexpected and crazy paths, just like real users do. To scale, we need to automate whatever we can, letting developers free of testing manually to focus on writing new features or improving the existing ones.

# What is an automated test?

As you may guess, an automated test is a test whose logic has been automated through code.

> 
 It's a piece of code written with the sole purpose of testing our production code, the code that is going to be used by our customers and users.

_** For the sake of simplicity and because it's widely used the word "test" to refer to an "automated test", we'll keep that correspondence on the rest of the article_

# Benefits of automated testing

* **Tests work as a safety net to make changes (refactoring)**: Now we feel confident on changing code because if we break something our tests will warn us
* **Tests can work as live documentation when they are descriptive enough**: Additional to documentation files now we can read our tests to understand how the code works and access to useful examples
* **Tests prevent wasting unnecessary time on debugging**: Rather than spending time trying to figuring out what is wrong, we use the feedback our broken tests will give us. If we have a bug, we write a new test to expose the error
* **Tests are the first users of our code**: We can have early feedback from our code, even before running the code if we write the tests before the code
* **Thinking in testing while coding can lead us to a better design**: Now we write our code to make it easier to test, to do so we rely on design patterns and best practices to build a decoupled code

[Here](https://mariocervera.com/non-obvious-benefits-automated-testing)  is a great article written by @[Mario Cervera](@macerub) to complement the benefits of testing.

# Example: A simple automated test

Suppose we need to write a code to reverse a string. That function will be used in our production code.

As we already know, a test is just code. Therefore, a test for the `reverse` function could look like the following function:

```JavaScript
import reverse from '../reverse'

function test(value, expected) {
    return reverse(value) === expected;
}
```
As we can see, a basic test just uses the real `reverse` function and compares its result against an expected result. We call that verification an _assertion_ or _expectation_.

With our test function written, we can start adding more tests cases:

* Reverse function must return the same string if it's a palindrome
```JavaScript
test('level', 'level');
```
* Reverse function must return the inverted string
```JavaScript
test('amazing', 'gnizama');
```
* Reverse string must return an empty string when receiving an empty string
```JavaScript
test('', '');
```

_**Now, what if we also want the reverse function to throw an error when receives a null or undefined value?**_

_How can we test that?_

We'll need to modify our `test` function to support the new requirement in the `reverse` function, the changes that we need to include are:
* Accept strings and errors as the second argument, the expected result
* Catch any error thrown by the `reverse` function and compare its error message with the message of the expected error

With the new requirements in mind, the resulting code of the `test` function will look like this:

```JavaScript
function test(original, expected) {
    if (expected instanceof Error) {
        return testError(original, expected);
    }
    return testString(original, expected);
}

function testString(original, expected) {
   return reverse(original) === expected;
}

function testError(original, expected) {
    try {
       reverse(original);
       return false;
    } catch(e) {
       return e.message === expected.message;
    }
```
With the `test` function updated, we can now add the new test cases:
* The reverse string function must throw an error when receives a null
```JavaScript
test(null, new Error('invalid value, expected an string'));
```
* The reverse string function must throw an error when receives an undefined
```JavaScript
test(undefined, new Error('invalid value, expected an string'));
```

Finally to know if the tests passed or not we will print the result in the terminal using the function `console.log`
```JavaScript
console.log(test('level', 'level'));
console.log(test('amazing', 'gnizama'));
console.log(test('', ''));
console.log(test(null, new Error('invalid value, expected an string')));
console.log(test(undefined, new Error('invalid value, expected an string')));
```

After running our tiny script we'll see something like the following

%[https://media.giphy.com/media/UguI0WAE3fAf2Y7Z57/giphy.gif]

**_Voila! We wrote our first test!_**. Although the feedback is really basic 🤔


# How PROS do it?

Short answer, as we did in the previous section.


%[https://media.giphy.com/media/mjwUZYo0Nduso/giphy.gif]

_it was a joke, of course not_

Remember that we said an automated test is just code? If the test is in charge of verifying the production code to work as expected, ** who checks the test to work as expected? **

Our tiny `test` function from the previous example was so smaller that we could easily identify a problem with our eyes.

But, also remember that as soon as we added another possible outcome, passing from returning a string to throwing an error, we needed to adapt the behavior of the `test` function to support that change without breaking the test.

Every time we are forced to add logic into the test because of a concern that's different from testing the production code, like extending the available assertions, the risk of committing an error increases. Imagine adding asynchronous code, numbers, timeouts, intervals, objects, and so on. Now it doesn't seem to be so easy to detect problems on our tests. 

To avoid any problem writing custom test functions, developers use libraries specifically designed to help writing tests.

Those libraries present utilities in the form of functions or classes to structure our tests, and that way, we focus on writing the logic inside. We can find them as third-party libraries or as a part of the programming language, like the _unittest_ package in Python.

Other libraries help us to deal with assertions. They supply utilities to compare different types of values, partially match objects or arrays,  match regular expressions, catching errors, handling promises, knowing the number of function calls, order of the function calls, and more. These libraries are called **_assertion libraries_**, and you can find them as a standalone library or as part of a whole testing framework.

![Screen Shot 2021-04-12 at 00.28.20.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1618201725482/HnBRnMMQF.png)
*In the above image **Describe**, **it** and **expect** are global functions set by the Jest library*

In JavaScript, a standalone assertion library is [_chai_](https://www.chaijs.com/) whereas [_Jest_](https://jestjs.io/docs/getting-started) includes its assertion utilities inside the whole framework.

There is another challenge when we write our own testing functions: _we need a way to run the tests written in different files_ , because in a big project having everything in one file is not optimal. Therefore we will need a way to find those test files, run them separately from our production code_, perhaps also in parallel to optimize time spent and execute a subset of tests when we need to.

Fortunately, there are command-line programs that can solve the problems mentioned before; those programs are called **_test runners_**.  In JavaScript  [*mocha*](https://mochajs.org/) , [*jest*](https://jestjs.io/docs/getting-started) and [*karma*](https://karma-runner.github.io/latest/index.html) are test runners.

We can execute our tests from a terminal with a test runner, allowing us to run them in Continuous Integrations pipelines to validate the code before merging it.

As we see, testing libraries provide us all the power we need to focus solely on writing the logic behind our tests.

Oh!, and the feedback improves drastically using libraries compared with the `test` function that we wrote in the prior section.
![Screen Shot 2021-03-31 at 00.15.38.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1617160696997/a_66ys_OD.png)

# How can we test?

## 1. Choose wisely the type of test

There are many types of tests, each one with different purposes and characteristics, but in general, we can classify them as **_functional tests_** and **_non-functional tests_**. The first exist to probe an expected behavior in our code, to check that what we coded is aligned with a functionality. The latter is for testing non-functional aspects of the system such as reliability, availability, scalability, and all the -ilities; they provide handy metrics to make data-driven decisions and to know when we need to optimize the code.

![tests_classification.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1618259791739/WCbuXDkEZ.png)

Functional tests provide us with feedback when working on the solution, while non-functional tests only after the solution worked, but it didn't satisfy a metric. Because of that, we'll likely have lots of functional tests and few non-functional tests in our project.

![different_levels_of_testing_with_logo.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1618285304580/0fxBvteQN.png)


The reason behind having different types of functional tests (unit tests, integration, end-to-end) is that each one focuses on different aspects of our code, testing components in isolation, testing how they integrate together, or testing the whole application including UI.  

Why are differences? Because when we require more integration(e.g., waiting until multiple services are up and running), the tests will be slower. With less integration, the feedback provided by the tests will be less confident regarding the whole system.

For example, let's said we are going to test a car's wheel, but we don't test if the wheel was bolted correctly to the chassis; even if the wheel passes its tests, the car could be bad ensembled because the wheel's tests don't tell us anything about it.

There is a concept called [ _"The testing pyramid"_](https://martinfowler.com/articles/practical-test-pyramid.html) used to balance how fast we get the feedback with how confidence it delivers us. The idea is to have as much as we can of isolated tests because they are faster and complement them with a few high-level tests involving multiples parts to test the application's crucial features and have high confidence.

## 2. Prepare the environment

After deciding which tests our system requires, the following questions can help us know more about what we need.

* Who will be in charge of writing those tests? Our team? another team? a QA team?
* The tests can be written completely isolated from the production code? Or do we need to have them as closes to the code we can to review the code and the tests together?
* Do we need to write the tests using a special syntax and file structure?

Answering the questions can guide us to decide whether we should write our tests in the same project or as a whole different project. 

For example, End-to-End tests use UI elements to detect changes in the layout and navigate the application under test; they don't use the code!!. Therefore, we can write those tests in a whole different project, but if we have to maintain those tests, do we have enough people to handle another project?. A better decision could be to include them as a folder inside the same project as the production code.


![test_folders.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1618466027739/q0zljtuwh.png)_Folder structure and file names for unit tests on JS and Python_

Finally, we cannot forget to read the documentation of the testing library or framework that we chose. Probably they will be configured to search a specific folder structure, filename, or even syntax. 

## 3. Anatomy of a functional test

Although there are different types of tests for various purposes, functional tests share a typical structure.


![anatomy2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1618467397924/HmWq-C-sp.png)

1. Import production code under test (not required in all the type of tests. e.g End-to-End)
2. Create a test suite (not required in all the languages neither libraries)
3. Write a test case
4. Write SetUp code
5. Call the production code
6. Assert the outcome

The above picture is a unit test, but the structure doesn't change too much in other functional tests. In each one, we'll have a setup to do everything needed to set the stage of the test case. Perhaps execute some actions or create data.

The setup can change from creating just objects to start up a database, create records on it, or even open a web page. The call to action could be a call to an API endpoint, and the assertion can be to compare if it has the correct status code.

How the structure is implemented can change from test to test, but its structure is more likely to remain the same.

### How we arrange the code in a test?
Steps four to six receive different names depending on the type of tests, but all means the same. It is like telling a linear story.

* _Introduction, Development, Conclusion_
* _SetUp, Call to action, Assertion_
* **_Arrange, Act, Assert_**
* **_Given, When, Then_**

# OK. They are great but, What about time? Will we go slower?

Yes, absolutely. At least at first, compared to not testing.

Every new skill requires time to develop. In this case, we need to change our mindset to start thinking about how the code we are writing can be tested. Also, learn to master the testing library or framework of choice.

At first it will appear that we are doubling our efforts by writing tests and production code. But, in reality, what was happening was that we were writing only half of the code required by not writing the tests.

> To write tests should be seen as part of implementing the feature or fixing the bug

Writing tests doesn't mean we'll go twice as slow. After building enough experience, we'll write tests much faster. Besides, we'll write automated tests just once and update them only when the behavior they're testing has changed.

Automated tests offer quick feedback (between milliseconds and seconds) and the opportunity to execute the tests as many times as we want without effort. We get all the benefits of testing but with zero-waste.

> 
By writing automated tests, we are betting on the long run; we seek stability.

In the end, it's a lot of time and money saved compared with debugging the code and having everyone busy handling defects without producing neither maintaining code.



# If are so great, why some devs avoid writing automated tests?

![1__6PGPKCLntCj43I1fyyAtw.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1618028754404/UB6rgU7QK.jpeg)

**-- Because it's not an easy task** 😟

Many things make writing tests a complex task.

* Not having the knowledge
* Fear of experimenting in an environment full of pressures (overload of work or unreal expectations and deadlines)
* Not having the culture
* Having a code so coupled and convoluted that it's tough to add tests

Everything worth the effort will be hard to achieve at the beginning because it demands discipline and consistency.

> 
Nothing is harder than working under a tight deadline and still taking the time to clean up as you go - Kent Beck, author of "Test-Driven Development by example" book.

# Conclusion

Writing automated tests is a necessary skill that all companies need to adopt to scale and survive in a fast-paced industry growing each day. 

Ensuring the quality of the products we build is a must to secure our customer's fidelity, and with the help of automated tests, we can do it without wasting the team's time. QA testers can use their time to automate and experiment proactively, and devs can use their time to build new features trusting that at any moment a test will break, delivering enough information to solve the problem as soon as something unexpected happens.

A good approach is to start with small, isolated, and fast tests. Then, complement with a few of slow and high confident tests.

Write tests using libraries and frameworks specifically designed for that. Also, organize the tests and code following their guidelines.

The whole idea of writing automated tests is to detect any defect immediately, moving the feedback more and more close to the development phase. But that doesn't mean we should discard manual tests completely; they are still a valuable tool for simulating unexpected user behavior that cannot be generalized either automated.

> 
The most important thing is gathering courage and losing the fear of going slow initially but knowing that things will improve eventually.


# What's next?

Thanks for reading until here. I hope I answered some of your questions, and much better if I raised questions you didn't know you have.

**This is the first of many articles about software testing. I'll explain different types of tests, teach how to write them, improve them to have clean tests, teach about Test-Driven Development, and much more.**

Finally, If you like, you can support me here 👇
