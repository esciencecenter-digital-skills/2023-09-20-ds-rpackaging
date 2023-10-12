![](https://i.imgur.com/iywjz8s.png)


# Week 2 - 2023-09-20 Reproducible Research with R Packages

Welcome to The Workshop Collaborative Document. This document is synchronized as you type, so that everyone viewing this page sees the same text.


## 🫱🏽‍🫲🏻 Code of Conduct

Participants are expected to follow these guidelines:
* Use welcoming and inclusive language.
* Be respectful of different viewpoints and experiences.
* Gracefully accept constructive criticism.
* Focus on what is best for the community.
* Show courtesy and respect towards other community members.

Report an issue or get in touch:
- b.vreede@esciencecenter.nl
- l.deboer@esciencecenter.nl
- training@esciencecenter.nl

## 🎓 Certificate of attendance

If you attend the full workshop you can request a certificate of attendance by emailing to training@esciencecenter.nl.

## ⚖️ License

All content is publicly available under the Creative Commons Attribution License: [creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/).

## 🙋Getting help

To ask a question, raise your hand in the Teams window, or ask a question in the chat.

The Teams channel 'Q&A' can be used between sessions to ask questions. We will periodically check the channel and respond, but we encourage you to help each other solve issues here as well!

## 🖥 Workshop website

💻 [Workshop website](https://esciencecenter-digital-skills.github.io/2023-09-20-ds-rpackaging/)

🛠 [Setup instructions](https://esciencecenter-digital-skills.github.io/2023-09-20-ds-rpackaging/#setup)

## 👩‍🏫👩‍💻🎓 Instructors

Barbara Vreede, Eva Viviani, Lieke de Boer

## 🧑‍🙋 Helpers

Dani Bodor, Flavio Hafner


## 🗓️ Agenda
Day 2. Wed 27 September 2023
|  Time | Topic                                    |
| -----:|:---------------------------------------- |
|  9:00 | Welcome & ice breaker                    |
|  9:15 | Recap & lessons learned (breakout rooms) |
|  9:45 | Recap & lessons learned (plenary)        |
| 10:15 | Coffee break                             |
| 10:30 | Testing                                  |
| 11:30 | Coffee break                             |
| 11:45 | Testing, Coverage                        |
| 12:45 | Wrap-up                                  |
| 13:00 | END                                      |

## 🔧 Exercises

### Exercise: what happened?
Run `usethis::use_testthat()` in the console, and take a look at its output. Also take a look at your folder system (press the refresh button if needed).

What happened?

#### Answer
Output:
```console
Adding `testthat` to Suggests field in DESCRIPTION
Adding "3" to Config/testthat/edition
Creating "tests/testthat/"
Writing "tests/testthat.R"
Call `use_test()` to initialize a basic test file and open it for editing
```

Couple the tests that we write to the functions we write. It's called unit testing because we are testing particular functions.

```r
usethis::use_test("make_groups")
```

This creates a new file `test-make_groups.R`. We can open it and click the button "Run Tests".
Then we see the output. A test can
- fail
- warn
- be skipped
- pass -- this is what we want


#### Exercise: what does this test do?
Open a newly created testfile (after running `usethis::use_test(<function>)`, you will find it in `/tests/testthat/`) and take a look at its contents. It should contain something like this:

```r
test_that("multiplication works", {
  expect_equal(2 * 2, 4)
})
```

This is just an example test, that was created to make your life easier. You can use it as a template for writing your own tests.

Take 10 minutes to:
- Figure out what is happening inside this test. Can you understand it?
- Write another test, just below this one, that checks that addition also works. Tip: you can copy-paste and edit the current test.


#### Answer

- "multiplication works" is a name. But when a specific test files, the name is useful for reading the output of the test call. It does not have to be name of the function we are testing. What helps is that it is unique and descriptive.
- the `expect` statements perform the test
    - `expect_equal` tests that two things are the same: (i) the operation `2*2`, (ii) the expected output (`4`)
- we can add any code within the `{}` brackets
    - various `expect` statements
    - any other code
    - R will just execute any code in there


```r
test_that("multiplication works", {
  expect_equal(2 * 2, 4)
  expect_equal(4 * 3, 12)
  expect_equal(2 * -2, -4)
  expect_equal(4 * 0, 0)
})

```

```r
test_that("addition works", {
  expect_equal(2 + 6, 8)
})
```

```r
test_that("addition works", {
  actual_addition <- 7 + 4
  expected_result <- 11
  expect_equal(actual_addition, expected_result)
  expect_true(actual_addition == expected_result)
  expect_false(actual_addition == expected_result)
  # expect_error
})
```

Moreover, using tests always uses the most recent version of the function. There is no need to install the package when we want to test the function.


#### Exercise: write your own test.
Now it is your turn to write a test. With a vector of 6 names as input, check that the output of `make_groups()` is a matrix with 3 rows and 2 columns.

Tip 1: tests can contain multiple assertions.
Tip 2: the functions `nrow` and `ncol` may come in handy.

Here is the documentation of all the `expect_*` functions: https://testthat.r-lib.org/reference/


#### Solution

```r
test_that("make groups divides groups equally", {
  groups <- make_groups(classmates)
  expect_equal(ncol(groups), 2)
  expect_equal(nrow(groups), 3)
})

```

**Tip**: with failing tests. It makes sure that the test *can* fail. Sometimes, tests will simply run (and pass), but without actually running the test/checking the condition that we want to test

Naming aspects: generic vs specific names (ie, the `groups` object)? Does not matter too much. What is important is whether the code is readable and understandable: does another person reading your code understand what the code is doing?



## :thought_balloon: Questions



### From the breakout rooms

✅ **Q: What is the difference between Rda and CSV files as part of the package?**
- differs what the kind of data is to share
    - to share an R object, share and Rda file. Allows users to quickly access that object
    - raw data (csv, ...) are put in  `inst/extdata`; users need to refer to that file with `sys.path`. the user needs another function to load that file into their environment
    - sharing a csv file makes it easy to open with any program/text editor.
- Example: `employees.csv` in `/inst/extdata`

```r
library(mysterycoffee)
system.file("extdata", "employees.csv", package = "mysterycoffee")
# refers to the specific file in the user's system

```
Summary of data use in a package:

![](https://hackmd.io/_uploads/HJpHq_vJp.png)

✅ **Q: What if functions are dependent on each other; do you have to put them into different files**
- depends on each case
- for instance, if two functions are used in the same context, it makes sense to have them in the same file. As in this example:

```r
make_groups <- function(names) {
  # Shuffle the names
  names_shuffled <- sample(names)

  # Arrange it as a two-columns matrix
  names_coupled <- matrix(names_shuffled, ncol = 2)
  return(names_coupled)
}

my_sample <- function(names) {
    return(sample(names))
}

```

It does not matter in which order the functions are put in the file. This is because the functions are not run yet (and when loading the package, all the functions are loaded simultaneously).

What if function name is the same as in another function?
- if `make_function` is also in another dependency of your package, it's best to rename your function if you can.
- important to refer to the function in the dependency by its full name. For example, if the package `grouping` is a dependency in our package, we call the `make_groups` function in grouping with `grouping::make_groups()`. This makes a clear distinction between the
- calling `make_groups` within your package will always first call the function from your package. more generally, R looks for a function with that name from very locally (in your package) to the environment (any loaded packages); as soon as it finds a function with that name, it uses that and stops looking.



✅ **Q: What if I have a specific purpose with my package, that is not generalizable?**
- function can be very tailored to your workflow (for instance, because it uses a very specific variable name in the dataset)
- this makes it not reusable for other people with other column names
- but it's still worth writing a package!
    - it's got a tidy structure
    - it's easy to test
    - it's easy for new users with the same dataset (ie, another researcher that works with you and the same data) to use the functionality of the package
- it's also good to make the package more robust and alert the user that a certain expected column name (that is expected by the package) does not exist in their input.

✅ **Q: If you're calling a function you defined within a package, do you access it with mypackage:: or can it be without?**
See the answer above.


✅ **Q: How do i use the pipe operator within my package function? magrittr::`%>%` doesn't work. Lieke mentioned using the usethis package.**

Use the native pipe.
    - the "pipe" is `%>%`. It's part of the `magrittr` package. But `magrittr::%>%` does not work
    - the "native pipe" is `|>`

```r
make_groups <- function(names) {
  names_coupled <- names %>%
    my_sample() %>%
    matrix(n_col = 2)
  return(names_coupled)
}
```

We just replace the pipe with the native pipe

```r
make_groups <- function(names){
  names_coupled <- names |>
    my_sample() |>
    matrix(ncol = 2)
  return(names_coupled)
}
```


✅ **Q: How can we use .Rbuildignore for the tester scripts, instead of using the untitled file?**
- in `.Rbuildignore`, add any file paths that are in your system but you do not want to be in the package infrastructure
- but don't overuse it


✅ **Q: Can you use testthat inside functions or in any other way, for other users to test their use of the function?**

The behavior between using `if` and the `test` is different. For a failing test, we directly get an error, which makes it difficult to play with the behavior. But with an `if` statement, we have more control over what happens if the condition fails.


```r
if(3 != 4) {
  stop("Error!")
}
```





❌ **Error:**
The code I try to test/run:
```r
test_that("multiplication works", {
  expect_equal(2 * 2, 4)
})

test_that("additional test", {
  expect_equal(2 + 2, 9)
})

test_that("uses tidyverse recycling rules", {
  expect_error(stringr::str_dup(1:2, 1:3), class = "vctrs_error_incompatible_size")
})
```

error:

```
==> Testing R file using 'testthat'



Loading myCoffee

Error in (get(".Internal", baseenv()))(delayedAssign(x, value, eval.env,  :

  cannot change value of locked binding for 'classmates'

Calls: <Anonymous> ... <Anonymous> -> export_lazydata -> copy_env_lazy -> delayed_assign

In addition: Warning message:

package 'testthat' was built under R version 3.6.3

Execution halted



Exited with status 1.
```

✅ **Q: I load data in my function with `data(my_data)`. Later in my function I want to delete my_data so that it wont show in the global environment. I added rm(my_data) at the end of my function. But I get the warning "object my_data was not found". How can I remove data loaded inside a function?**

- Good example of unintended behavior of the function.
- You can check what happens while you execute the function by looking at the `Environment` tab

```r
make_groups <- function(names) {
  data("starwars_names")
  names_coupled <- names |>
    my_sample() |>
    matrix(ncol = 2)
  return(names_coupled)
}
```
Calling the function loads the data into the global environment. Instead of doing this, it's better to assign the data to a variable in the local environment. This makes sure that the object is deleted at the end of the function.

When such data is necessary as part of the package, then it's better to access the object directly:

```r
pick_classmates <- function() {
  return(sample(classmates, 1)) # no data(classmates) here!
}
```


✅ **Q: how can I get my editor theme to also apply to the testing output? I like that it's green when the test works on Barbara's RStudio :)**

Don't know :/

✅ **Q: Can you only work with what a function returns, or can you also access objects created within a function? as a test mid-way so to say?**

- Single operations inside a function that are not returned themselves cannot be tested directly. But this is a good reason to split larger functions into smaller parts (separate functions), and then one can test each of these subfunctions.


:question: **Q: What actually happens when you push the 'Run Tests' button? does it run testthat.R? and then?**

Will check for next time.



✅ **Q: I see that there can be multiple environments within packages. Can you explain how these are used?**

- Objects created within a function are only available within the function (inside the scope of the function)
- If we call an object in a function that was created not in the scope of the function, R looks for the object in other places. Ie, in the package scope, or even in the global environment.




## 🧠 Collaborative Notes

### Welcome / Recap
- "Q&A" button in teams for questions -- let us know if you don't see that button
- why is an R package a package?
    - it has a very specific structure
    - code should only consist of functions -- this is what the user will interact with
- today: testing
    - will make the package robust

```r
install.packages("testthat")
install.packages("covr")
install.packages("DT")
# or in one
install.packages(c("covr", "testthat", "DT"))
```

### Testing

What are tests? unit tests? integration tests?
- make sure that the functions do what you want them to do
- make sure that all functions in the package are still functioning (when adding new things)

```r
library(mysterycoffee)
classmates
make_groups(classmates) # this is a way of testing the function: we can inspect whether the output is what we expect
```

Manual testing like the above quickly becomes hard to manage. It's more efficient to formalize this into a test. Then, the computer will do the checks for us.


```r
make_groups(1)
make_groups(NA)
```


#### Data and testing
We can also create specific data that we want to use in tests.

```r
myVector <- c("1", "2", "3", "4")
save(myVector, file = "tests/testthat/myVector.rda")

```
This vector will not be available when the package is installed. But we can use it in the tests:

```r

load("myVector.rda")
groups <- make_groups(myVector)

test_that("make_groups divides groups equally") {
    expect_equal(ncol(groups), 2)
    expect_equal(nrow(groups), 2) # this will fail
}

```

### Testing, Coverage
We need the `covr` and `DT` package for this.

```r
covr::report()
```
This calculates the "coverage"  in the package: fraction of lines of code that are executed during a test.

What are cases when a call to a function does not lead to 100% coverage of the function (because not all lines are covered)?

Example

```r
make_groups <- function(names) {
  var <- 4
    names_coupled <- names |>
    my_sample() |>
    matrix(ncol = 2)
  return(names_coupled)
}
```

```r
make_groups <- function(names, result = TRUE) {
  if (!result) {
    return("there is no result") # this line is never called in the current tests
  }
  names_coupled <- names |>
    my_sample() |>
    matrix(ncol = 2)
  return(names_coupled)
}
```

In `test-make_groups.R`:


```r
test_that("false flag works on make_groups", {
  groups <- make_groups(myVector, result = FALSE)
  expect_equal(groups, "there is no result")
})

```

Better coverage does not imply that the test is good. Good testing is much a skill that grows with experience.

output of `covr::report()`: red line -- does this mean we have to do something about it? Not necessarily. But especially in larger packages, it's good to have a high coverage.


```r
test_that("sample returns something", {
    cm <- pick_classmates()
    expect_type(cm, "character")
    expect_equal(length(cm), 1)
})
```

Main advice for testing: start with failing tests, then build the function until it passes the test.
- makes sure the test can fail
- speeds up workflow


## 📚 Resources

- Documentation of all the `expect_*` functions: https://testthat.r-lib.org/reference/


### Homework after week 2
In this coming week, we would like you to apply the things you learned today to your own package. Start with the following steps:
1. Setup test infrastructure for your package with `usethis::use_testthat()`
2. Create a test using `usethis::use_test("test-context")`

Then, please write tests for one or more of your functions. You can do this as you see fit. For example:
- Write multiple tests for a single function, to test that the function deals well with multiple kinds of input. Remember you can put multiple assertions in a single test (an assertion is stating an expectation, e.g. `expect_equal`, `expect_true`).
- Check if your function produces specific error messages (using e.g. `expect_error`).
- Write a test for (part of) your workflow, in which multiple functions are used (remember that this is called *integration testing*).
- Use `covr::report()` to identify parts of your package that could benefit from more tests.

As you do this, here are some useful links:
- The collaborative document for today: https://tinyurl.com/rpackages-week2
- Testthat documentation (including a description of all expect_ functions): https://testthat.r-lib.org/reference/index.html

Finally, and most importantly, do continue to develop the functions in your package. You can do this together with newly developed tests: your tests can help you confirm that functions still work as expected while you edit (refactor) them.