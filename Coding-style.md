R Development
=============

A lot of the code for PEcAn is written in R, for more information about
R see the primary references (sources) of content herein:

* [Writing R Extensions](http://cran.r-project.org/doc/manuals/R-exts.html)
* [Hadley Wickham's Wiki (Book)](https://github.com/hadley/devtools/wiki/Introduction) 
* [questions tagged "r" at Stack Overflow](http://stackoverflow.com/questions/tagged/r?sort=votes&pagesize=50)
* [Google`s R Style
Guide](http://google-styleguide.googlecode.com/svn/trunk/google-r-style.html)

Coding Style
------------

Consistent coding style improves readability and reduces errors in
shared code.

R does not have an official style guide. Google has one that is well
thought out and widely adopted. [Google`s R Style
Guide](http://google-styleguide.googlecode.com/svn/trunk/google-r-style.html)
is a quick read and valuable reference. It has been used as a reference
since the PEcAn`s early development.

### Use Roxygen2 documentation

This is the standard method of documentation used in PEcAn development,
it provides inline documentation similar to doxygen. Even trivial
functions should be documented.

See [[Roxygen2]] Wiki page

### Write your name at the top

Any function that you create or make a meaningful contribution to should
have your name listed after the
`author tag in the function documentation.

#### Use testthat testing package

* tests provide support for documentation - they define what a function is (and is not) expected to do 
* all functions need tests to ensure basic functionality is maintained during development.
* all bugs should have a test that reproduces the bug, and the test should pass before bug is closed
* See [[Unit_Testing|Testing]] wiki for instructions

#### Don't use shortcuts

R provides many shortcuts that are useful when coding interactively, but that can cause problems when written into packages. 

For example, the following are equivalent:

```{r}
mydata <- data.frame(X = 1, Y = 2)
# Good
plot(mydata$X, mydata$Y)
# Bad:
# which() and attach() are useful in interactive mode 
attach(mydata)
plot(X, Y)

with(mydata, plot(X, Y)
```


#### Function Names

By arbitrary decree, we decided early on to use the all lowercase with periods to separate words. They should generally have a verb.noun format, such as query.traits, get.samples, etc.

#### File Names

File names should end in .R and should be meaningful. For example, they should be named after the primary functions that they contain. There should be a separate file for each major high-level function. This aids in identifying the contents from a list of file names

#### Use "<-" as an assignment operator

* Because most R code uses <- (except where = is required), we will use <-
* "=" is used for function arguments
* examples:
```{r}
# appropriate use of "="
plot(x = 1, y = 2)

# appropriate use of "<-"
X <- 1
Y <- 2
plot(X, Y)
```

#### Use Spaces

* around all binary operators (=, +, -, <-, etc.). 
* after but not before a comma

#### Use curly braces

The option to omit curly braces is another shortcut that makes code easier to write but harder to read and more prone to error.

```{r}
* Good:
myfn <- function(x, y) {
ans <- sum(x, y)
return(ans)
}

if(this) {
  a <- 1
} else {
  a <- 2
}

* Bad:
myfn <- function(x,y)
    sum(x,y)

if(this) 
  a <- 1
 else 
  a <- 2
```

#### Package Dependencies: library vs require vs DEPENDS


When another package is required by a function or script, it can be called in the following ways:

1. As a package dependency loaded with the package
   This should be the default approach when writing functions in a package. There can be some exceptions, for example when a rarely-used or non-essential function requires an esoteric package. 
2. using `library`
   if dependency is not met, will print an error and stop
3. using `require`
   will print a warning and continue (but will throw an error when a function from the required package is called) 

It is considered best practice to use DEPENDS and SUGGESTS in DESCRIPTION; SUGGESTS should be used for packages that are called infrequently, or only in examples and vignettes; suggested packages are called by require inside a function.

From p. 6 of the "R extensions manual":http://cran.r-project.org/doc/manuals/R-exts.html

> The `Suggests` field uses the same syntax as `Depends` and lists packages that are not necessarily needed. This includes packages used only in examples, tests or vignettes (see Section 1.4 [Writing package vignettes], page 26), and packages loaded in the body of functions. E.g., suppose an example from package foo uses a dataset from package bar. Then it is not necessary to have bar use foo unless one wants to execute all the examples/tests/vignettes: it is useful to have bar, but not necessary. Version requirements can be specified, and will be used by R CMD check.

Reference: Stack Overflow ["What is the difference between require and library?"](http://stackoverflow.com/questions/5595512/what-is-the-difference-between-require-and-library)

#### Error handling 

(To be updated by Rob)
