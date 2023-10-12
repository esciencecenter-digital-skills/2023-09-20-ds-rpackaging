![](https://i.imgur.com/iywjz8s.png)


# Week 1 - 2023-09-20 Reproducible Research with R Packages

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



## 🖥 Workshop website

💻 [Workshop website](https://esciencecenter-digital-skills.github.io/2023-09-20-ds-rpackaging/)

🛠 [Setup instructions](https://esciencecenter-digital-skills.github.io/2023-09-20-ds-rpackaging/#setup)

## 👩‍🏫👩‍💻🎓 Instructors

Barbara Vreede, Eva Viviani, Lieke de Boer

## 🧑‍🙋 Helpers

Dani Bodor, Flavio Hafner


## 🗓️ Agenda
Day 1. Wed 20 September 2023
|  Time | Topic                                             |
| -----:|:------------------------------------------------- |
|  9:00 | Welcome                                           |
|  9:15 | Introduction, Getting started, Accessing packages |
| 10:15 | Coffee break                                      |
| 10:30 | Writing own functions, Licenses                   |
| 11:30 | Coffee break                                      |
| 11:45 | Data                                              |
| 12:45 | Wrap-up                                           |
| 13:00 | END                                               |

## 🔧 Exercises

#### Exercise: edit the function, and reload the package.
The default package generates `Hello, world!` when the only function`hello()` is called.

1. Change this function to generate a different output. (Have fun with it, perhaps add an argument or two!)
2. Build and install the package, and call the function again to see that it works differently.


#### Exercise: tell me how you load your functions?
1. How do you usually work with (your own) functions?
   A. Do you source them from an external file?
   B. Do you usually work on a single, long script that includes the functions?
   C. .... other (please share!)
2. What would be the advantages of using a package instead?

- A
- I source the functions from scripts that contain only functions.
- A, advantage: you can share functions over projects (and with different people)
- A, When changing functions you only have to change once if you reuse the code again
- A
- A, in that way I can make adjustments to all functions (have them next to each other to check if they're consistent) (and not make accidental typos into a function thats in my working script)
- B(70%), A(30%)
- 1. A 2. Accessible from any computer if you put it on e.g. GitHub, can be a group project with multiple authors.
- combination of A&B. Package is easier for version control and to have everything together for future reference or for others to use.
- A and B depending on length of function
- A
- Mostly A, but I also deal with B at work, which is not ideal
- A
- Modularity, organization


#### Exercise: adding data
1. Create an object that you want to include in your package (for example, a vector of names)
2. Save the object as data in your package
3. Remove the object from your environment (you can use the Rstudio user interface for this, or the function `remove()`)
4. Install and reload your package. Confirm that you can still access your object!

Share your code below.

```r
starwars_names <- sample(starwarsdb::people$name, 10)
usethis::use_data(starwars_names)
```


#### Exercise: adding raw data
1. Use an existing raw data file (an excel, csv, or other file), or create one (for example, save the content below as `names.csv`)
```name,age,function
anders,28,sales
bert,44,hr
christine,61,ceo
dina,28,manager
emma,44,sales
frank,30,logistics

```
2. Save the file in your package in `./inst/extdata`
3. Install and reload the package.
4. Confirm that you can access the file:
```r
filepath <- system.file("extdata", "names.csv", package = "mysterycoffee")
names <- read.csv(filepath)
```

Share a :thumbsup: when you're done, or paste your error message below.


## :thought_balloon: Questions

### Question: inst folder
So just to check: the system.file by default looks for a folder called specifically 'inst' (so it cannot be named differently) and it looks there for the rest of the path?

*Answer*: Good question, and the answer is: yes, and no :)
- inst is a folder that you create *in the package you write*, which means that it exists as you are editing your package
- when that package is installed, the content of inst is moved to the root of the installed package.
- inst cannot be called differently!

*Response*: thank you, that is clear. I was wondering indeed how this is moved while installing, how does the installation 'know' where to put it/what it is. But it knows it then because it's originally always in a folder called 'inst'

### Question: sandboxes
Does it create any problem if I have my experimental scripts/sandbox in the project folder? Or should I rather keep them elsewhere?

*Answer*: in short, the package should be as clean as possible.
Options:
- Untitled and unsaved files
- Use an external project
- Have a script, but add it to your .Rbuildignore (this way it won't get included when the package is installed)


## 🧠 Collaborative Notes


### Creating a package
Open R

We will start from a clean project (you can add existing code to this later):
- File > New Project
- New Directory
- R Package
- Give it a name
  - for our dummy project: "mysterycoffee"
  - but can be any name for your project
- If you already had files open, use: Open in new session

You have now created a package. But what is a package?
- a folder with:
  - a folder called R for your \*.r files (your codebase)
  - a folder called man (for manual)
  - a NAMESPACE file (we will get back to this later)
  - a DESCRIPTION file (lots of info about the package, we will also get back to this later)

On the left top panel there's a tab called "Build". Click on that and then on the button "Install"
By clicking Install, the session is started in the console.

To update the package, you don't only have to update the function, but also install it again

To use a function from the package rather than a function from the environment, specify this by first using the name of the package, then two semicolons (::), then the function.
E.g., `mysterycoffee::hello("YourName")` will not necessarily work the same as `hello("YourName")`, depending on whether the function in the environment and package are identical.


### Calling functions

On the importance of being explicit. If you use the package calling method above then you can be sure which function you are calling, whereas if you just use the function name it is less clear which package it comes from function.
On the "Packages" tab, you can see all the available (external) packages and which ones are installed (and install/uninstall).

You can use external packages inside your own package. The correct way to do this is to NOT load external packages (library or attach functions), instead use the double colon notation mentioned above.

```r
hello <- function() {
    person <- sample(starwarsdb::people$name, 1)
    print(paste("Hi", person))
}
```

In some cases you might need dependencies where you do need to load external packages (we will cover this in a future session). Even in that case, never call the external package from _inside_ the function.

Installing and accessing packages is not the same. Installing packages (also specifying versions) is necessary, but the way you access them is best done as described above (as opposed to importing the library).

### DESCRIPTION file
We will now start updating the DESCRIPTION file.
- Title: should describe what the package does.
  - Simulation of random encounters between couples of persons
- Version: (leave it like this for now)
- Author: change to your name (you!)
- Maintainer: again you (you can add your email address)
- Description: should be a longer description, multiple lines, detailing what it does. (IMPORTANT: when using multiple lines, it needs to be indented (four spaces))
```
Description: Simulate random encounters between people (in couples)
    The idea of this package is to simulate random encounters among people to get coffee, when there is no such an office.
```
- License: (we'll get back to this)
- Encoding: leave it as is (unless you have a good reason to change)
- LazyData: keep it as `true` (unless you have a good reason to change)

Now re-install your package.


### Semantic versioning:
A naming convention that tells you which stage a package is at. A combination of 3 numbers, separated by dots: major.minor.patch
Using a 0 as major version indicates that it is still in development. When you are ready to "release" the package (when it's ready for use), then you can update the major version to 1.
See more info on semantic versioning: https://semver.org/


### Populating your package
Only use functions you actually need in your package. So you can delete the `hello` function that was added by default.

We will create our first function, to simulate people meeting for coffee:

```r
make_groups <- function(names) {
    # 1st step: shuffle the names
    names_shuffled <- sample(names)

    # 2nd step: arrange it in a two-column matrix
    names_coupled <- matrix(names_shuffled, ncol = 2)

    # 3rd step: it has to return the names coupled
    return names_coupled
}
```

To make the function available, re-install the package.
On the console:

```console
library(mysterycoffee)
make_groups(c("Eva", "Barbara", "Dani", "Fenne"))
```
#### On cases in naming conventions...
![](https://cdn.myportfolio.com/45214904-6a61-4e23-98d6-b140f8654a40/dbb99049-2916-4bc8-824f-1816f5c4f06d_rw_1920.png?h=f0b45a30ba31ad414562d1085cd6c172)
[Source: Alison Horst](https://allisonhorst.com/everything-else)


### Licensing
Two main categories of licenses
- Permissive
  - code can be freely copied, modified, published
  - well known examples: Apache, MIT, BSD
- Copyleft/viral:
  - more restrictive/strict on what can be done with it
  - examples: GPL, GNU

How to decide which license to use:
- Are you using external code (i.e. copying that code, not just using it as a dependency)? In that case it may depend on the license of that code.
- Is there an official policy of your employer/university/organization/granting agencies, etc.
- Always use a license. Not using a license means: NO ONE CAN USE YOUR CODE (including you yourself).
- If you don't know which license to use, go to https://choosealicense.com/.

At the eScience Center, we usually use Apache 2.0 licenses (very open and permissive). This is what we will use for this project as well.
On your console run: `usethis::use_apache_license()`. This will create the license for you, change the field in the DESCRIPTION file and add a LICENSE.md file to your package.

How to see the license for a package (e.g. ggplot2): `packageDescription("ggplot2", fields = "License")`

After updating your DESCRIPTION file, if you look at the available packages, you will see the new title and version number.


### Working with data
We will make a script now (not just console). This script is not something we'll save, but it's useful to play around in.

In the script:
```r
library(mysterycoffee)

classmates <- c("Eva", "Dani", "Barbara", "Fenne")
make_groups(names = classmates)
```

Now on console
```console
usethis:use_data(classmates)
```
Now the `classmates` dataset will be stored in your project in the newly created data folder as a \*.rda file, the main/easiest data file for r packages.

Now remove the classmates line from the script, but still call classmates from the make_groups function. This script will now run fine, because classmates is stored in your package.

Especially if you include larger datasets, make sure that LazyData is set to true in the DESCRIPTION, as this means that the data is only loaded when/if it is used (avoiding unnecessary clogging of user environment).

For big datasets, it can be useful to use external places to store data (e.g. zenodo) that can be loaded from the package.

Using csv files in the package (rather than as an rda file). This cannot go into your data folder, as that only allows for rda files. Rather you can store these in a folder called "inst/extdata"

calling a csv file called employees.csv from your script:

```r
# locally on your own package
filepath_1 <- "inst/extdata/employees.csv"
read.csv(filepath_1)
```

This only works if you're currently working inside the package. Others that want to call use this data, but may be working from a different package (this only works if the package is installed.)

```r
# from installed package
filepath_2 <- ("extdata", "employees.csv", package = "mysterycoffee")
system.file(filepath_2)
```
Note that the file location is slightly different than above.



## 📚 Resources

Link to introduction slides: https://github.com/esciencecenter-digital-skills/2023-09-20-ds-rpackaging/blob/main/files/Netherlands_eScience_Center_intro_slides.pptx

Upcoming events: https://www.esciencecenter.nl/events/?f=workshops

Signing up for the eScience Center Newsletter: http://eepurl.com/dtjzwP

Semantic versioning: https://semver.org/

R-packages by Hadley Wickham and Jennifer Bryan: https://r-pkgs.org/
Part of this about package version number: https://r-pkgs.org/lifecycle.html#sec-lifecycle-version-number

All packages available on r: https://cran.r-project.org/web/packages/available_packages_by_name.html


## :house::school: Homework after week 1
Create a package using your own R project, if you have one (see the last point below if you do not have one). Create functions in the R folder of that package.

Remember the following:

* When you are using external functions inside your package, add double colons `::` before calling that function
* If you start from a script, you can keep the leftovers (if there are any after separating out functions) in a script that calls your package's functions and runs your workflow.
* It is very good if **at least one of your functions gives an output** (so, it doesn't just "do" something, like print `Hello World`): it actually creates a data frame, or a vector, or an object of some other kind! We will see why next week.
* Don't forget through `Install (and Restart)` whenever you change anything.
* If you are creating your own package, don't forget to update the `DESCRIPTION` file.
* If you are not using your own project, take some time to improve the current `make_groups` function and perhaps add another function.

Don’t worry if you run into issues, this is expected. See if you can fix them, but don't be afraid to bring them next week, so we can solve them together.

