![](https://i.imgur.com/iywjz8s.png)


# Week 4 - 2023-09-20 Reproducible Research with R Packages

Welcome to The Workshop Collaborative Document. This document is synchronized as you type, so that everyone viewing this page sees the same text.

----------------------------------------------------------------------------


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

Flavio Hafner


## 🗓️ Agenda
Day 4. Wed 11 October 2023
|  Time | Topic                                    |
| -----:|:---------------------------------------- |
|  9:00 | Welcome & ice breaker                    |
|  9:15 | Recap & lessons learned (breakout rooms) |
|  9:45 | Recap & lessons learned (plenary)        |
| 10:00 | Coffee break                             |
| 10:15 | Vignettes, wrap-up                       |
| 11:15 | Coffee break                             |
| 11:30 | Sharing packages                         |
| 12:30 | Wrap-up                                  |
| 12:45-13:00 | END                                      |

## 🔧 Exercises

#### Exercise: explore vignettes
Take some time to explore the vignette(s) of your favorite package(s). What things do you notice? What properties of vignettes stand out to you?

Recall that you can use these functions to look at vignettes from packages you have installed:
```r
browseVignettes(package = "dplyr")
vignette("in-packages", "dplyr")
```

Share your observations below!

- There is not always a vignette in a package
- Sometimes there is more than one vignette
- For me it opens a pdf rather than an R help page
- vignette is in doc folder
- some vignettes are pdfs, not html files, and then they open as pdf when called from console
- I found a course-like document
- there's a lot of examples that I used to google
- you can only see the vignette if the package is installed
- in-packages argument doesn't seem to work all the time
- i can only open the vignette within Rstudio, not on the internet (through link opened in browseVignettes)




Thus,
- Vignettes are not mandatory, but a good way to inform users about purposes and usage of the package
    - Ie, also tutorials/teaching purposes. See for instance "tidy data" from the `tidyr` package
- The vignette is written in Rmarkdown, and we can render it to html as well as pdf
    - they are opened in different ways because they are rendered in different ways
    - moreover, we can add executable code to the document. This is nice because one can then also show the output of the code.
- Vignettes are available in two ways
    - Online on CRAN (without installing the package)
    - When the package is installed, the vignettes are stored on the computer and we can always access them (see functions above)
-




### Exercise 2

- Use `usethis::use_vignette(vignette-name)` to create a vignette fit for your package.
- In the .Rmd file that is now created, add a code chunk that uses a function from your package. (Feel free to add some descriptive text, too!)
- Preview the vignette with the Knit button.
- Create the vignette with `devtools::build_rmd("vignettes/vignette-name.Rmd")`
- Verify that the vignette was built: a `.html` file should now appear in the vignettes/ folder.


### Share your package (final exercise)
As the final exercise today, we will share the packages we created with each other.

- On your GitHub profile, add a new repository (with the name of your package).
- Upload the entire package to the repository.
- Ensure your README file contains:
    - Instructions to install the package: `devtools::install_github("username/repository")` will install the package from github.
    - Note that if there are vignettes, you need to request their installation explicitly: `devtools::install_github("username/repository", build_vignettes = TRUE)`
    - A first function or small script to run (or: a link to the Vignette that contains this).
- Place the link to the package in the table below.
- Find a package from another user, install it and run their example. Comment BELOW the table (otherwise it will become a mess).




## :thought_balloon: Questions

:question: **Q: i think in the last week we will look at sharing packages. for me personally it would be very interesting if you could also share some insights in maintaining a package. If many people use your package, how do you handle and communicate changes in your functions?**

**Answer**
- github issues to communicate with users and developers
- pull requests allow other people to contribute to your code (and vice-versa) -- for instance to fix bugs or add features
- the same functionality is also available on gitlab


**What do you do when you change the functionality of a function?**
You may be familiar with changing functions, for instance the `tidyr::spread`.
But changing the functionality can have important impacts on the users, so need to be careful when doing this.

How can one communicate changes?
- in the function documentation
- through version numbers -- see earlier content of the course
    - for instance, if you making a change so that something stops working, you should update the major version number (from 1.2.3 to 2.0.0).
    - more generally, changes that alter behavior should bump the version number

It's also good to minimize changes to version number: change funtionality without making old code break. Example

From this:
```r
make_groups <- function(names) {
    # some code
}
```

To this
```r
make_groups <- function(names, times= c("9:00", "12:00")) {
    # some code
}
```


✅ **making dependencies available in the whole package (instead of calling `package::function()` every time the function is used in the package)? where can we import them?**

**Answer**:
`usethis::use_import_from("dplyr", "mutate")` creates a new file `packagename-package.R`. This file then makes a Roxygen skeleton and makes the function available across the package.
Advantages:
- Saves code in the function definitions
- this workflow also automatically updates other files in the package

Disadvantage:
- It may create problems when we have two dependency functions with the same name.




## 🧠 Collaborative Notes

### Creating a vignette

```r
usethis::use_vignette("example") #name of the vignette
```

What does it do?
- adds dependencies: `knitr`, `rmarkdown`
    - they are in the `Suggests` section of DESCRIPTION because the functionality of the package does not depend on them
- creates `vignettes/example.Rmd`
- updates `.gitignore` (stuff that we do not want to version-control)

Content of the rmarkdown file:

```{r setup}
library(mysterycoffee)
```

Here is how to use it:
```{r usage example}
make_groups(classmates)
```

Note the naming of the cell:
- the mandatory part is `{r}`, which tells Rmarkdown which language the cell is written
- a nice-to-have is a descriptive name of the call, such as "usage example" above


We can create an html file of the vignette with
```r
devtools::build_rmd("vignettes/example.Rmd")
```
Or, we can also press the "Knit" button in RStudio.
We can then manually open the vignette html file. But the following will not work yet:

```r
browseVignettes("mysterycoffee")
# No vignettes found by browseVignettes("mysterycoffee")
```

Instead, we need to install the package as follows:

```r
devools::install(build_vignettes = True)
```
```r
browseVignettes("mysterycoffee")
#works
```

Note -- `devools::install(build_vignettes = True)` is enough for the workflow to work; we do not need to create the html file first. But creating the html file is handy while we are writing the vignette because we can interactively inspect the resulting vignette without having to install the full package.


### Using git

Step-by-step to set up git in make a first commit
- Install git (see [link](https://git-scm.com/downloads))
- Initialize a new git repository
- In Rstudio, there should be a Git tab nex to the build tab
- Tick all files so that there's an green "A" next to them
- click 'commit'
- write git commit message
- press "commit" button

Now, whenever we change something in any file in the directory, after saving the change becomes visible in git. We can then again add this file to git and commit with a message.

Git is a great way to keep track of the project history because we can always go back to past states of the project and see changes we made. One can link a git repository to github and upload changes from the local repository to github. But it's also fine to use github without git by manually dragging-dropping files directly to github.

### Setting up github

- go to www.github.com
- after logging in, on top right there's a button with a plus sign. click on "new repository"
- fill in the fields
    - name of the repository -- does not need to be the same name as your local repo/your package. but it's good to keep order
    - **make sure to leave everything else empty**: no README, no .gitignore, no licence
    - if possible, use a public repository. but choose "private" if there's data that you don't want to share publicly.
- then press "create repository"

Copy the url to the repository ("Quick setup -- if you/ve done this kind of thing before"). If you've github before, use ssh. otherwise https.

Github shows options on how to add local code to that repo. see for instance "push an existing repository from the command line"


#### Linking to github
(a) with the command line
```bash
git remote add origin url/of/the/repository.git
git branch -M main
git push -u origin main
```

(b) Alternatively, use Rstudio as follows
- Tools > Version Control > Project setup > Git/SVN tab
- then, paste the repository url to the field "Origin"


(c) Drag-and-drop
To clearly separate from the above approach, we use a new repository.
Create a new one as above, naming "mypackage_dragdrop" or similar. Choose "public"--since we use drag and drop, we never accidentially upload files we do not want to share.

There should be a link "upload existing file". Then go into the mysterycoffee directory on you computer, select the files you want to uplad, and drag them into the github window in your browser. this should then upload the selected files to github, preserving the directory structure (ie, "tests/test_myfunction.R", "R/myfunction.R")

then, commit.

#### Telling users on how to download your package

In github, open the README file to edit and add this information:

without vignettes:
```r
devtools::install_github("bvreede/mysterycoffee_dragdrop")
# pattern: username/packagename
```


with vignettes:
```r
devtools::install_github("bvreede/mysterycoffee_dragdrop", build_vignettes = True)
# pattern: username/packagename
```

#### Using other people's package

Now, you  can install a package of one of the other course participants with these instrutions. (For this, open R in a new, empty project, not in your package, and run the above commands in there.)


### Insights
- there are other system-level dependencies (Stan) that the user needs to install before the package works
- in just a few minutes, it was possible for other people to use your code (with or without errors). thus, packaging lowers the access barriers to your code and makes it much easier for other people to user your code.




## 📚 Resources
[Carpentries material about learning Git](https://swcarpentry.github.io/git-novice/)
[Good practices in software engineering](https://www.esciencecenter.nl/event/good-practices-in-research-software-development-2/)
Download [Git](https://git-scm.com/downloads)
Ho to set up [SSH key](https://docs.github.com/en/github-ae@latest/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) on github

Using git and github with R:
- https://happygitwithr.com/rstudio-git-github
- https://rfortherestofus.com/2021/02/how-to-use-git-github-with-r/ (tutorial with videos)
- https://support.posit.co/hc/en-us/articles/200532077
- https://hansenjohnson.org/post/sync-github-repository-with-existing-r-project/

