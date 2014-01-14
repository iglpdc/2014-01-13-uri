#Testing and debugging

*Inspired largely by Karthik's Canberra materials.*

## Outline

- Why testing? 
- The most popular bug in science
- How to prevent bugs
- How to test code: Unit testing in R
- Writing solid functions
- S@%t happens: how to debug

## Why testing?

Common sense tells you that things should be tested before use them for real.
Otherwise, things may go wrong. However, some people seem to disagree with this
view. They think that:

- you can land a vehicle in Mars [without testing your
software](http://mars.jpl.nasa.gov/msp98/news/mco991110.html),
- you can shape
global economy  [without testing your
software](http://www.businessweek.com/articles/2013-04-18/economists-spreadsheet-error-upends-the-debt-debate),
- you can shoot missiles [without testing your
software](http://www.ima.umn.edu/~arnold/disasters/patriot.html).
  
*Don't be that guy!* **Test your software!**

## The most popular bug in science: division gone bad 

You got a big simulation going to build the next generation of submarines. At
some point deep in your code, you divide its weight by its length to see
whether it will float:

```
> 3/2
```

The result is 1.5, *right?* In most programming languages, including Python,
C/C++, Fortran, the computer will output `3/2 = 1`! Your design parameters are
off by 50%, and your [submarine
sinks.](http://www.telegraph.co.uk/news/worldnews/europe/spain/10073951/2-billion-Spanish-navy-submarine-will-sink-to-bottom-of-sea.html)

The computer is doing nothing wrong. This is valid code and the expected
result. In those programming languages, if you divide two integers the `/`
operator means integer division and `3/2 = 1`. 

**Test your software.**

## Why people don't test code?

When we learn to code, we write functions so simple that don't
need to be tested. Also, instructors use the class time to show more
new stuff instead of spending time writing tests. In general, testing
appears as wasted time. 

However, [profesional programmers spend **90%** of their time fixing bugs](http://programmers.stackexchange.com/questions/3241/what-amount-of-time-should-be-spent-on-bugs-vs-original-development). So,
if you prevent just half of your bugs, your productivity will double!

## How to prevent bugs

How about **testing your code?**. In fact, there are three things that turn buggy
code into solid code:

- [Document your
code](https://github.com/jhollist/2014-01-13-uri/blob/gh-pages/rLessons/Functions.md)
- [Keep your code under version control](../06-version-control/)
- Test your code, specially against corner conditions.

## How to test code: Unit testing in R

One way of testing code is testing all the functions that compose your code against
as all the possible conditions that they will be facing. If all the functions work
by themselves, when your put them together into a program or a script, the
whole thing has no points of failure. This way of testing programs is called
unit testing. 

You can do unit testing in R using a package called `testthat`. The hierarchy of the package consists of:  

* expectations
* tests 
* and contexts. 

Expectations are the basic functions you will use. You can group them in tests
and group tests in contexts. 

The expectations are:

**1. `equals()` Equality with a numerical tolerence**
````
# passes
expect_that(10, equals(10))
# passes
expect_that(10, equals(10 + 1e-7))
# Fails
expect_that(10, equals(10 + 1e-6))
# Definitely fails!
expect_that(10, equals(11))
```

**2. `is_identical_to`:  Exact quality with identical**

```
expect_that(10, is_identical_to(10))
expect_that(10, is_identical_to(10 + 1e-10))
```

**3. `is_equivalent_to()` is a more relaxed version of equals()**

```
# ignores attribute names
expect_that(c("one" = 1, "two" = 2), is_equivalent_to(1:2))
```

**4. `is_a()` checks that an object inherit()s from a specified class**  

```
model <- lm(mpg ~ cyl, mtcars)
expect_that(model, is_a("lm"))
```


**5. `matches()` matches a character vector against a regular expression.**

```
string <- "Testing is fun!"
# Passes
expect_that(string, matches("Testing"))
```

**6. `prints_text()` matches the printed output from an expression against a regular expression**

```
a <- list(1:10, letters)
# Passes
expect_that(str(a), prints_text("List of 2"))
# Passes
expect_that(str(iris), prints_text("data.frame"))
```


**7. `shows_message()` checks that an expression shows a message**

```
expect_that(library(mgcv),
shows_message("This is mgcv"))
```

**8. `gives_warning()` expects that you get a warning**

```
expect_that(log(-1), gives_warning())
expect_that(log(-1),
  gives_warning("NaNs produced"))
# Fails
expect_that(log(0), gives_warning())
```

**9. `throws_error()` verifies that the expression throws an error. You can also supply a regular expression which is applied to the text of the error**

```
expect_that(1 / 2, throws_error())
expect_that(seq_along(1:NULL), throws_error())
```


**10. `is_true()` is a useful catchall if none of the other expectations do what you want -it checks that an expression is true**

```
x <- require(ggplot2)
expect_that(x, is_true())
```

This entire suite of tests can also be shortened.

| Long form | Short form |
| -------   | ---------  | 
| expect_that(x, is_true()) | expect_true(), expect_false() |
| expect_that(x, is_true()) | expect_true(), expect_false() |
| expect_that(x, is_a(y)) | expect_is(x, y) |
| expect_that(x, equals(y)) | expect_equal(x, y) |
| expect_that(x, is_equivalent_to(y)) | expect_equivalent(x, y) |
| expect_that(x, is_identical_to(y)) | expect_identical(x, y) |
| expect_that(x, matches(y)) |  expect_matches(x, y) |
| expect_that(x, prints_text(y)) | expect_output(x, y)  |
| expect_that(x, shows_message(y)) | expect_message(x, y) |
| expect_that(x, gives_warning(y)) |  expect_warning(x, y) |
| expect_that(x, throws_error(y)) |  expect_error(x, y) |


### Writing a test suite

Basic syntax

```
test_that("a description for the test suite", {
# load all necessary objects and variables
# ...
# run tests
# run more tests
})
```

A full example


```
context("String length")

test_that("str_length is number of characters", {
  expect_that(str_length("a"), equals(1))
  expect_that(str_length("ab"), equals(2))
  expect_that(str_length("abc"), equals(3))
})
test_that("str_length of missing is missing", {
  expect_that(str_length("NA"), equals(2))
})
test_that("str_length of factor is length of level", {
  expect_that(str_length(factor("a")), equals(1))
  expect_that(str_length(factor("ab")), equals(2))
  expect_that(str_length(factor("abc")), equals(3))
})
```


### Running tests

Save all tests in a folder. In the case of a package, the folder is located under `inst/tests`. Otherwise save into a file that has the word `test` in the title. example. `test_script.R`

Run using one of the following:

```
test_file("test-str_length.r")
test_dir('path/to/test/folder')
test_dir('path/to/test/folder', "minimal")

source("test-str_length.r")
```

### Load and test package

```
library(testthat)
library(package_name)
test_package("package_name")
```

### Automatic testing

You can leave another instance of R to run tests automatically when code is changing rapidly. This can be done using the `autotest()` function. It takes:

* A code path
* and a test path
* (optional) reporter = "summary" by default. Can be changed to "minimal".

The minimal reporter prints:  
* **‘.’** for success, 
* **‘E’** for an error 
* and **‘F’** for a failure. 


## Writing solid functions

One way to force yourself to write documentation and tests for your code is to
write them *before* writing any actual code. This is actually a programming
practice enforced in many software companies, and it's called test-driven
development. 

Suppose that you want to write a function that does a certain thing. 

- First of all, you write a sentence stating what exactly this function does.
This will be the short description at the beginning of the function's
documentation. Then, you write the documentation for its arguments, using
`@param`. Then, you write the documentation for the return value, using
`@return`. 

- Second, with this information you can write the *interface* of the function,
that is, the part that is outside the curly brakets: name and list of
arguments. Pick a name that fits your sentence. Don't write anything in the
function's body yet.

- Thrid, as you know the name of the function, its arguments, and its return
value, you can write some tests. Go for tests picking weird situations, and
other for which you know the result for sure by other method. Usually, you will
find that you have to make some decisions about your function that you didn't
think about it. E.g. what is gonna do if an argument is a `NA`: ignore it or
throw an error?

- Fourth, write the main part of the documentation stating all the decisions
that you have made writing the test. E.g. if the argument is a `NA` it print a
warning message, and keep going.

- Last, once you are happy with your tests, i.e. they cover all the different
situations your function may encounter, write the body of the function. The only
thing you need is a function that passes your tests. It will be extremely easy
because you have pretty much state clearly what you need to do.

Finally, don't write very long functions. If a function takes more that 10-15
lines, it can be probably be splitted into smaller functions. Do it! Short
functions are much easier to read and test, so you will have less bugs and more
fun.

### Exercise

Write a rock-solid function that solves a quadratic equation, i.e. `ax^2 + bx +
c = 0`.  The specifications for this function are: it takes 3 parameters, the
number `a, b, c`, and returns a vector with two numbers, the solutions of the
equation. Also:

- if the `a = 0`, the equation has only one solution; your function must throw
the error `I'm linear`.

- if `b^2 - 4ac < 0`, the solutions are complex numbers. We don't like that, so
your function must throw the error `Getting complex`.

Write the documentation, a few tests covering the situations above, and post
your function as a [gist](https://gist.github.com/) in your Github account.

## S@%t happens: how to debug

We are not going to lie, even with all this, bugs will happen. *Often*. When you
have a bug, you should use a tool, called debugger, to track it down and fix it. 

What makes bugs hard to find is that they are valid code, so the computer is
happy to run it. It's just the code doesn't do what you want it to do. 

A debugger is a tool that executes you code line by line, allowing to inspect
all the variables that your computer is using, to see where it's doing
something you are not expecting.

R incorporates a debugger which is mostly managed by two R functions.

If an error happens:

```
traceback()
```

will print the trace of your program, that is the chain of call that caused the
error. You can use `traceback()` to see precisely where your code fails.

Once you know where you code fails, you should inspect what's going on:

```
browser()
```
allows you to inspect the stack, i.e. all the values of all the variables in
the computer's memory. 

Once in the browser, you can move line by line in your code by hitting `n`,
inspecting how the variables involved in the bug change as the code executes.
To quit the browser hit `q` and to execute many lines at once hit `c`. 

Once you find your bug, fix your code and analyze why the bug appeared. Then,
write a test that prevents this bug to happen again. As they say, *a bug is just
a test that wasn't written*.
