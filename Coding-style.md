R Development
=============

A lot of the code for PEcAn is written in R, for more information about
R see
[http://cran.r-project.org/doc/manuals/R-intro.pdf](http://cran.r-project.org/doc/manuals/R-intro.pdf)

Coding Style
------------

Consistent coding style improves readability and reduces errors in
shared code.

R does not have an official style guide. Google has one that is well
thought out and widely adopted. [Google’s R Style
Guide](http://google-styleguide.googlecode.com/svn/trunk/google-r-style.html)
is a quick read and valuable reference. It has been used as a reference
since the PEcAn’s early development.

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
# Good:
myfn <- function(x, y) {
    ans <- sum(x, y)
    return(ans)
}

if(this) {
  a <- 1
} else {
  a <- 2
}

# Bad:
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

> The ‘Suggests’ field uses the same syntax as ‘Depends’ and lists packages that are not necessarily needed. This includes packages used only in examples, tests or vignettes (see Section 1.4 [Writing package vignettes], page 26), and packages loaded in the body of functions. E.g., suppose an example from package foo uses a dataset from package bar. Then it is not necessary to have bar use foo unless one wants to execute all the examples/tests/vignettes: it is useful to have bar, but not necessary. Version requirements can be specified, and will be used by R CMD check.

Reference: Stack Overflow ["What is the difference between require and library?"](http://stackoverflow.com/questions/5595512/what-is-the-difference-between-require-and-library)

#### Error handling 

(To be updated by Rob)

### Data in a package 

#### Summary:

Files with the following extensions will be read by R as data:
* plain R code in .R and .r files are sourced using `source()@ \
\* text tables in .tab, .txt, .csv files are read using `read()`\
**\* \
** objects in R image files: .RData, .rda are loaded using `load()`\
**\* capitalization matters \
**\* all objects in foo.RData are loaded into environment\
**\* pro: easiset way to store objects in R format\
**\* con: format is application ® specific (discussed in \#318)

Details are in `?data`, which is mostly a copy of [Data section of
Writing R
Extensions](http://cran.r-project.org/doc/manuals/R-exts.html#Data-in-packages).

### Accessing data

Data in `/data/` directory will be accessed in the following ways,

\# efficient way: (especially for large data sets) using the `data`
function:\

    data(foo) # accesses data with, e.g. load(foo.RData), read(foo.csv), or source(foo.R) 
    ```
    # easy way: by adding the following line to the package DESCRIPTION:
      note: this should be used with caution or it can cause difficulty as discussed in #1118
    ```{r}
    LazyData: TRUE

From the R help page:

Currently, four formats of data files are supported:\
\# files ending ‘.R’ or ‘.r’ are ‘source()’d in, with the R working
directory changed temporarily to the directory containing the respective
file. (‘data’ ensures that the ‘utils’ package is attached, in case it
had been run *via* ‘utils::data’.)\
\# files ending ‘.RData’ or ‘.rda’ are ‘load()’ed.\
\# files ending ‘.tab’, ‘.txt’ or ‘.TXT’ are read using ‘read.table(…,
header = TRUE)’, and hence result in a data frame.\
\# files ending ‘.csv’ or ‘.CSV’ are read using ‘read.table(…, header =
TRUE, sep = “;”)’, and also result in a data frame.

If your data does not fall in those 4 categories you can use the
system.file function to get access to the data:\

    system.file("data", "ed.trait.dictionary.csv", package="PEcAn.utils")
    [1] "/home/kooper/R/x86_64-pc-linux-gnu-library/2.15/PEcAn.utils/data/ed.trait.dictionary.csv"

The arguments are folder, filename(s) and then package. It will return
the fully qualified path name to a file in a package, in this case it
points to the trait data. This is almost the same as the data function,
however we can now use any function to read the file, such as read.csv
instead of read.csv2 which seems to be the default of data. This also
allows us to store arbitrary files in the data folder, such as the the
bug file and load it when we need it.

#### Examples of data in PEcAn packages

-   issue \#1060 added time constants in
    source:utils/data/time.constants.RData
-   outputs: source:/modules/uncertainties/data/output.RData
-   parameter samples source:/modules/uncertainties/data/samples.RData

Packages used in development
----------------------------

### roxygen2

Used to document code. See instructions under [[R\#Coding\_Style|Coding
Style]]

### devtools

Provides functions to simplify development

Documentation:
[https://github.com/hadley/devtools](https://github.com/hadley/devtools)

    load_all("pkg")
    document("pkg")
    test("pkg")
    install("pkg")
    build("pkg")

other tips for devtools (from the documentation):

Adding the following to your \~/.Rprofile will load devtools when
running R in interactive mode:

    ## load devtools by default
    if (interactive()) {
      suppressMessages(require(devtools))
    }
    ``` 

    Adding the following to your .Rpackages will allow devtools to recognize package by folder name, rather than directory path


    ```{r}
    # in this example, devhome is the pecan trunk directory 

    devhome <- "/home/dlebauer/R-dev/pecandev/"
    list(
        default = function(x) {
          file.path(devhome, x, x)
        }, 
      "utils" = paste(devhome, "pecandev/utils", sep = "")
      "common" = paste(devhome, "pecandev/common", sep = "")
      "all" = paste(devhome, "pecandev/all", sep = "")
      "ed" = paste(devhome, "pecandev/models/ed", sep = "")
      "uncertainty" = paste(devhome, "modules/uncertainty", sep = "")
      "meta.analysis" = paste(devhome, "modules/meta.analysis", sep = "")
      "db" = paste(devhome, "db", sep = "")
    )

Now, devtools can take “pkg” as an argument instead of “/path/to/pkg/”,
e.g. so you can use `build("pkg")` instead of `build("/path/to/pkg/")`
