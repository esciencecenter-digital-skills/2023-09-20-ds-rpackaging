![](https://i.imgur.com/iywjz8s.png)


# Week 3 - 2023-09-20 Reproducible Research with R Packages

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
Day 3. Wed 4 October 2023
|  Time | Topic                                    |
| -----:|:---------------------------------------- |
|  9:00 | Welcome & ice breaker                    |
|  9:15 | Recap & lessons learned (breakout rooms) |
|  9:45 | Recap & lessons learned (plenary)        |
| 10:15 | Coffee break                             |
| 10:30 | Dependencies, Documenting                |
| 11:30 | Coffee break                             |
| 11:45 | Documenting, Vignettes                   |
| 12:45 | Wrap-up                                  |
| 13:00 | END                                      |

## 🔧 Exercises

#### Exercise: Specify a minimum version to your dependencies
1. run `?usethis::use_package` and find out how to specify a minimum version required for your package, i.e., which argument you need to use within `usethis::use_package`?
2. specify a version number, e.g., "3.0.0".
3. EXTRA: Rather than specifying a number, try to set it to `TRUE` and report what has happened.


#### Exercise: run 'Check'
1. Run the 'Check' function (either with the button, or with `devtools::check()`).
2. Look for information about your dependencies in the resulting text. Are any dependencies missing, or declared dependencies unused?
3. How are you going to solve the problem? Please take some minutes to try to solve this problem by yourself.

in console:
```r
usethis::use_package("dplyr", type = "Imports", min_version = TRUE)
```


#### Exercise: Edit your documentation entry
Take 5 minutes to edit the skeleton with real documentation. In particular:
1. Add a title.
2. Describe, below the title, what the function does.
3. Describe the parameter names.
4. Describe the output that is returned.
5. Ignore @export.
6. Delete @examples.

Share your documentation skeleton, including the first line of your function (namely: `functionName <- function(param1, param2){`), below.


## :thought_balloon: Questions

✅ **Q: i think the course gives a good foundation, but maybe it would also be nice to see some more advanced examples of packages and tests (for me personally). now we have a small example package, maybe we can view one of your own bigger/mature packages? for example, which expect_ functions do you use most?**
- things to look for in a nice package (e.g.):
  - good READMEs with goal, install instructions, usage instructions, etc
  - extensive tests
  - continuous integration on GitHub (outside scope of course) that checks e.g. specific combinations of operating systems and R versions
  -
- see e.g. [kinematics package](https://cran.r-project.org/web/packages/kinematics/index.html) as a nice "complete" package
- intermediately mature package (shown as example in course): [MEAanalysis](https://github.com/ankemt/MEAanalysis)

✅ **How can I decide what to test?**
- look for "smallest unit" that performs a specific function/task, or that has an input/output. Write a test specifically for that. Then look for the next thing.
- Making a test for an entire (long) script can become very complex, because if it does not work, it is difficult to troubleshoot why. At this point try to test something smaller instead.
- Look for something that you know should have a specific (type of) input/output, write a test for that. This includes testing that something that should not work, throws an error.

✅ **Did anyone else run into the problem with covr that we saw last week? The error message says the package is in use and will not be installed.**

- try to detach your package/ unload your package and re-try
- I also encountered this problem. I think if there was failure in the test, it will report such error. Covr::report（) work if the test has all passed.
- Lieke: In this case the tests all passed.

✅ **How do we make sure that our functions are as robust as possible?**
- they should be as robust *as necessary*. Otherwise, this becomes unwieldly.
- use tests (a lot). Be creative with how you test. Not just testing what works, but also that what should not work indeed does not work (e.g. test for errors).
- when a "weird" error pops up: when solved also write a test that verifies that that error does not re-occur in the future.

✅ **Why do booleans not work in test_that, while test_equal does?**
- test_that does not use booleans, but expect functions (e.g. `expect_type`, `expect_equal`, `expect_true`, etc)

✅ **In Python you can cluster tests inside classes according to functionality. Is this possible in R?**
- You can use a file as a way to sort them, classes are not needed.
- Write seperate `test_that` functions to organize the tests.
- This is a different paradigm than in Python

❓ **I want to have in my package a function to create all plots that I use in my paper. However, one of the inputs for one plot has 230 MB. The users can't create this input themselves because it requires a long computational time. Is it ok to add a file of that size to the package? if not, is there a way around?**
- You can link to an external location of this file; e.g. on figshare or zenodo, depending on the type of file this is.
- Some advice: you can use arguments (like: `download = FALSE`) to make sure that users do not accidentally download the file when they do not want to, and have to EXPLICITLY indicate that they want to download the file.

❓ **Is there a way to list all required packages as dependencies in one shot?**
- no

❓ **Difference between mandatory vs suggested package?**
- Think from the point of view of the user rather than the script. Users won't need all the packages that you need when writing the script.
- Suggested dependencies might be used in "edge cases", uch as development. For example:
  - users won't generally need `testthat`, or dependencies that are only used while testing
  - only used in one function for specific cases.

❓ **I have a function that make a plot using ggplot. I have added ggplot as a dependency. But when I try to use the function, I get the error "could not find function "aes" ". What is going wrong here?**
- Check this vignette: https://ggplot2.tidyverse.org/articles/ggplot2-in-packages.html which discusses some strategies!


❓ **On minimizing the number of dependencies**
- You are likely to need dependencies
- It's a developers choice on when to use a dependency as opposed to write the function yourself
- No need to re-invent the wheel, just to avoid using a dependency
- But having less dependencies is nice, so simple things can be written out
- If you have a dependency, then it is vital that you explicitly state the dependency (with version numbers if necessary) in the DESCRIPTION file (you can use `usethis::use_package()` for this)


## 🧠 Collaborative Notes

### Dependencies

A dependency is when a tool/function/class/etc requires another one to work

**Take home message**
Dependencies have 2 goals:
1. making your package robust
2. communicating how your package works

"Mandatory" dependencies are requirements to run the package. Not installing them will break your package, meaning that (some) functions will not work or not work as intended.

"Suggested" dependencies are a bit more nuanced. They are used for specific things, mainly developing the package. The package/functions will not break or fail without the suggested dependencies.

Dependencies are listed in your DESCRIPTION file. Generally writing them manually is error prone. Instead you can use `usethis` in the console, e.g.

```r
usethis::use_package("testthat", type = "Suggests")
```

This will automatically add the dependency, in the correct formatting, to your DESCRIPTION file.

Add a mandatory package like this (press up key to show the previous command in the console):
```r
usethis::use_package("ggplot2", type = "Imports")
```

You can look at specifics of how to list dependencies, including version numbers, etc (The question mark at the beginning of a command in the console shows the help function for a command.):
```r
?usethis::use_package
```


Specifying specific version of ggplot:

```r
usethis::use_package("ggplot2", type = "Imports", min_version = "3.4.2")
```

or, to use the version in your current system (even if that is a virtual environment)

```r
usethis::use_package("ggplot2", type = "Imports", min_version = TRUE)
```

Other types of dependencies (other than "Imports" and "Suggests") exist as well. However, these are the main two that you need, the other types are for very specific situations.

You can press the "Check" button, which will make RStudio check whether all the dependencies are installed. This will also give you information of whether the dependencies are actually used in your package.

Removing packages from dependencies (unlike adding them) is easily done manually, by deleting the line. This is not error prone, because deleting a linewill usually not result in formatting errors.


Make a function to specify time to meet
```r
make_groups_by_time <- function(names){
    # input - character vector containing names
    # output - dataframe with re-arranged names in groups by time

    # step 1. make groups (using previously defined function)
    groups <- data.frame(make_groups(names))

    # step 2. rename the columns of groups
    names(groups) <- c("person1", "person2")

    # step 3. decide the times when people can meet
    possible_times <- c("09:30", "10:00", "14:00", "16:30")

    # step 4. combine possible times with groups
    groups_by_time <- dplyr::mutate(groups,
                                    time = sample(possible_times,
                                                  size = nrow(groups),
                                                  replace = TRUE)
    )

    # step 5. return output
    return(groups_by_time)
}
```
note: you can select all and hit Ctrl+i / Cmd+i to indent everything correctly.


To show groups plus meeting times, run in the console:
```r
make_groups_by_time("Eva", "Lieke", "Barbara", "Dani")
```



### Documentation

Removing the default documentation by deleting the folder called "man" and the NAMESPACE file.
Check in your packages whether you have roxygen2 (at least approx version 7.something). If not, run `install.packages("roxygen2")`.

Go to "Build" tab, "More", "Configure Build Tools..."
Tick the box: "Generate documenatation with Roxygen". Then in configure, tick all except "Vignettes".

Generating a documentation skeleton. Go to your `make_groups` function, or your own package, or any other function. Put the cursor inside the function you want to document. Then in the command bar go to "Code" and then "Insert Roxygen Skeleton". This will add the skeleton documentation for the function


```r
#' Group people in pairs
#'
#' Turn a vector of names into a matrix with randomly
#' paired people, preparing for a myestery coffee
#' date!
#'
#' @param names (character) vector with names of participants
#' @param result Boolean flag, indicating whether a result should be returned
#'
#' @return matrix of coupled names
#' @export
make_groups <- function(names, result = TRUE){
    # etc
}
```

Note that the title doesn't need to be the function name. It can (should) be more descriptive.
`@param`s are the parameters that you already have in the function. You can annotate them here
`@return`: be clear about the format
`@examples`: can be very useful, but is outside of the scope of this course. Feel free to look up what it is for

When you install the package, you will see that the "man" folder and "NAMESPACE" file have re-appeared (auto generated).

Check whethe it renders nicely by running the help for the function: `?make_groups`

Now run Build to check that the documentation errors have disappeared.


##### Documenting data

1. Create a new R file called `data.R` using `usethis::use_r("data")` (`data.R` is an example; you may call this whatever you want).
2. In this file, document the data object, using this example:
    ```
    #' Starwars names
    #'
    #' A vector of 10 randomly chosen names from the Starwars universe.
    #'
    #' @format character vector
    #' @source The mind of George Lucas? \url{www.starwars.com}
    "starwars_names"
    ```
3. Call `devtools::document()` to generate the documentation file and add this data to the package namespace!
4. Install and verify that your documentation can be accessed with `?data` (or `?anothername` if your data object is called differently).

Fields (in order of lines):
- Title
- empty line
- Description
- empty line
- `@format`: what format is data in?
- `@source` Where did it come from?

**Take home message**: roxygenize always works



If the `@export` flag is removed, then it is not added to the environment. This is useful if a function is only intended for "internal" use, i.e. not accessed by user, only by the other functions in the package.



## 📚 Resources

Muzak: https://en.wikipedia.org/wiki/Muzak
Importing ggplot: https://ggplot2.tidyverse.org/articles/ggplot2-in-packages.html
Book on R packages by Hadley Wickham and Jennifer Bryan : https://r-pkgs.org/dependencies-mindset-background.html


## :house::hammer: Homework
As you perhaps already expect, the homework for this week is… write documentation for your package! Some additional tips and points of attention:

- Make sure you are clear on what functions are for the user — label them with `@export` — and what functions are only used internally.
- For the exported functions: insert a Roxygen skeleton (Code > Insert Roxygen Skeleton), and fill it out. Do explore the Roxygen documentation (https://roxygen2.r-lib.org/) and experiment with different tags.
- After documenting the package (e.g. with `devtools::document()` or `roxygen2::roxygenise()`), explore what the help pages for your functions look like, by pulling them up as you normally would with `?function_name`. Adjust as necessary.

Aside from the Roxygen documentation, do write a README.md file with some basic information about your package! This will be the first thing potential users see, so what do you want them to know? How can they get started?
If you have not generated the readme yet, generate it as follows:
`usethis::use_readme_md()`.
(Note that you do not need to add the installation information yet, we will do this together next week.)

Finally, make sure the dependencies of your package are clearly stated in the DESCRIPTION file. Note also that running a Check will only check your code for functions that are labeled with `packagename::function_name()`, and it will not detect any functions from external packages if they are not labeled correctly. Go through your code to be extra sure that functions are called correctly and you are not missing dependencies!

Remember that next week we will share our respective packages. Your package will not be complete and that is completely expected and perfectly OK. However, it would be nice if your package has at least one function that works and is well documented!

