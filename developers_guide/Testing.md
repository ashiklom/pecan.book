PEcAn uses the testthat package developed by Hadley Wickham. Hadley has
written instructions for using this pakcage in his
[Testing](http://adv-r.had.co.nz/Testing.html) chapter.

Rationale
---------

*  makes development easier
*  provides working documentation of expected functionality
*  saves time by allowing computer to take over error checking once a
    test has been made
*  improves code quality
*  Further reading: [Aruliah et al 2012 Best Practices for Scientific
    Computing](http://arxiv.org/pdf/1210.0530v3.pdf)

Tests makes development easier and less error prone
---------------------------------------------------

Testing makes it easier to develop by organizing everything you are
already doing anyway - but integrating it into the testing and
documentation. With a codebase like PEcAn, it is often difficult to get
started. You have to figure out

*  what was I doing yesterday?
*  what do I want to do today?
*  what existing functions do I need to edit?
*  what are the arguements to these functions (and what are examples of
    valid arguments)
*  what packages are affected
*  where is a logical place to put files used in testing

Quick Start:
------------

* decide what you want to do today
* identify the issue in redmine or github (if none exists, create one)
* to work on issue 99, create a new branch called “redmine99”,
“github99” or some descriptive name… Today we will add enable an
existing function, `make.cheas` to make `goat.cheddar`. We will know
that we are done by the color and taste.

    ```bash
    git branch goat-cheddar
    git checkout goat-cheddar
    ```
    * open existing (or create new) file in `inst/tests/`. If working on code in "myfunction" or a set of functions in "R/myfile.R", the file should be named accordingly, e.g. "inst/tests/test.myfile.R"
    * if you are lucky, the function has already been tested and has some examples.
    * if not, you may need to create a minimal example, often requiring a settings file. The default settings file can be obtained in this way:
      ```r
      settings <- read.settings(system.file("extdata/test.settings.xml", package = "PEcAn.utils"))
      ```
    * write what you want to do
      ```r
      test_that("make.cheas can make cheese",{
        goat.cheddar <- make.cheas(source = 'goat', style = 'cheddar')
        expect_equal(color(goat.cheddar), "orange")
        expect_is(object = goat.cheddar, class = "cheese")
        expect_true(all(c("sharp", "creamy") %in% taste(goat.cheddar)))
      } 
      ```
    * now edit the goat.cheddar function until it makes savory, creamy, orange cheese.
    * commit often
    * update documentation and test
      ```r
      library(devtools)
      document("mypkg")
      test("mypkg")  
      ```
    * commit again
    * when complete, merge, and push
      ```bash
      git commit -m "make.cheas makes goat.cheddar now"
      git checkout master
      git merge goat-cheddar
      git push
      ```

Test files
----------

Many of PEcAn’s functions require inputs that are provided as data.
These can be in the `/data` or the `/inst/extdata` folders of a package.
Data that are not package specific should be placed in the PEcAn.all or
PEcAn.utils files.

Some useful conventions:

### Settings

* A generic settings can be found in the PEcAn.all package
```r
settings.xml <- system.file("pecan.biocro.xml", package = "PEcAn.BIOCRO")
settings <- read.settings(settings.xml)
```
*  database settings can be specified, and tests run only if a connection is available

We currently use the following database to run tests against; tests that require access to a database should be enclosed by `if(db.exists())` to avoid failed tests on systems that do not have the database installed.


```r
settings$database <- list(userid = "bety", 
                          passwd = "bety", 
                          name = "bety",     # database name 
                          host = "localhost" # server name)
if(db.exists()){
  test_that(... ## write tests here
}
```
*  instructions for installing this are available on the [VM creation
    wiki](VM-Creation.md)
*  examples can be found in the PEcAn.DB package (`db/inst/tests/`).

* Model specific settings can go in the model-specific module, for
example:

```r
settings.xml <- system.file("pecan.biocro.xml", package = "PEcAn.BIOCRO")
settings <- read.settings(settings.xml)
```
* test-specific settings:
 * settings text can be specified inline:
 
  ```r
  settings.text <- "
  <pecan>
  <nocheck>nope</nocheck> ## allows bypass of checks in the read.settings functions 
  <pfts>
    <pft>
      <name>ebifarm.pavi</name>
      <outdir>test/</outdir>
    </pft>
  </pfts>
  <outdir>test/</outdir>
  <database>
    <userid>bety</userid>
    <passwd>bety</passwd>
    <location>localhost</location>
    <name>bety</name>
  </database>
</pecan>"
settings <- read.settings(settings.text)
 ```
 * values in settings can be updated:
```r
settings <- read.settings(settings.text)
settings$outdir <- "/tmp" ## or any other settings
```

### Helper functions created to make testing easier

*  **tryl** returns FALSE if function gives error
*  **temp.settings** creates temporary settings file
*  **test.remote** returns TRUE if remote connection is available
*  **db.exists** returns TRUE if connection to database is available

When should I test?
-------------------

A test *should* be written for each of the following situations:

1.  Each bug should get a regression test.
 * The first step in handling a bug is to write code that reproduces the error
 * This code becomes the test
 * most important when error could re-appear
 * essential when error silently produces invalid results

2.  Every time a (non-trivial) function is created or edited
 * Write tests that indicate how the function should perform
     * example: `expect_equal(sum(1,1), 2)` indicates that the sum
            function should take the sum of its arguments

 * Write tests for cases under which the function should throw an
        error
  * example: `expect_error(sum("foo")`
  * better : `expect_error(sum("foo"), "invalid 'type' (character)")`

What types of testing are important to understand?
--------------------------------------------------

### Unit Testing / Test Driven Development

Tests are only as good as the test

1.  write test
2.  write code

### Regression Testing

When a bug is found,

1.  write a test that finds the bug (the minimum test required to make
    the test fail)
2.  fix the bug
3.  bug is fixed when test passes

How should I test in R? The testthat package.
---------------------------------------------

tests are found in `~/pecan/<packagename>/inst/tests`, for example
`utils/inst/tests/`

See attached file and
[https://github.com/hadley/devtools/wiki/Testing](https://github.com/hadley/devtools/wiki/Testing)
for details on how to use the testthat package.

#### List of Expectations

|Full	|Abbreviation|
|---|----|
|expect_that(x, is_true())	|expect_true(x)|
|expect_that(x, is_false())	|expect_false(x)|
|expect_that(x, is_a(y))	|expect_is(x, y)|
|expect_that(x, equals(y))	|expect_equal(x, y)|
|expect_that(x, is_equivalent_to(y))	|expect_equivalent(x, y)|
|expect_that(x, is_identical_to(y))	|expect_identical(x, y)|
|expect_that(x, matches(y))	|expect_matches(x, y)|
|expect_that(x, prints_text(y))	|expect_output(x, y)|
|expect_that(x, shows_message(y))	|expect_message(x, y)|
|expect_that(x, gives_warning(y))	|expect_warning(x, y)|
|expect_that(x, throws_error(y))	|expect_error(x, y)|

#### How to run tests

add the following to “pecan/tests/run.all.R”

```r
library(testthat)
library(mypackage)

test_package("mypackage")
```

### basic use of the testthat package

Here is an example of tests (these should be placed in
`<packagename>/inst/tests/test_<sourcefilename>.R`:

```r
test_that("mathematical operators plus and minus work as expected",{
  expect_equal(sum(1,1), 2)
  expect_equal(sum(-1,-1), -2)
  expect_equal(sum(1,NA), NA)
  expect_error(sum("cat"))
  set.seed(0)
  expect_equal(sum(matrix(1:100)), sum(data.frame(1:100)))
})

test_that("different testing functions work, giving excuse to demonstrate",{
  expect_identical(1, 1)
  expect_identical(numeric(1), integer(1))
  expect_equivalent(numeric(1), integer(1))
  expect_warning(mean('1'))
  expect_that(mean('1'), gives_warning("argument is not numeric or logical: returning NA"))
  expect_warning(mean('1'), "argument is not numeric or logical: returning NA")
  expect_message(message("a"), "a")
})
```


#### Script testing

It is useful to add tests to a script during development. This allows
you to test that the code is doing what you expect it to do.

```r
* here is a fake script using the iris data set

test_that("the iris data set has the same basic features as before",{
  expect_equal(dim(iris), c(150,5))
  expect_that(iris$Sepal.Length, is_a("numeric"))
  expect_is(iris$Sepal.Length, "numeric")#equivalent to prev. line
  expect_is(iris$Species, "factor")
})

iris.color <- data.frame(Species = c("setosa", "versicolor", "virginica"),
                         color = c("pink", "blue", "orange"))

newiris <- merge(iris, iris.color)
iris.model <- lm(Petal.Length ~ color, data = newiris)

test_that("changes to Iris code occurred as expected",{
  expect_that(dim(newiris), equals(c(150, 6)))
  expect_that(unique(newiris$color),
              is_identical_to(unique(iris.color$color)))
  expect_equivalent(iris.model$coefficients["(Intercept)"], 4.26)
})
```


#### Function testing

Testing of a new function, `as.sequence`. The function and documentation
are in source:R/utils.R and the tests are in source:tests/test.utils.R.

Recently, I made the function `as.sequence` to turn any vector into a
sequence, with custom handling of NA’s:


```r
function(x, na.rm = TRUE){
  x2 <- as.integer(factor(x, unique(x)))
  if(all(is.na(x2))){
    x2 <- rep(1, length(x2))
  }
  if(na.rm == TRUE){
    x2[is.na(x2)] <- max(x2, na.rm = TRUE) + 1
  }
  return(x2)
}

```
    

The next step was to add documentation and test. Many people find it
more efficient to write tests before writing the function. This is true,
but it also requires more discipline. I wrote these tests to handle the
variety of cases that I had observed.

As currently used, the function is exposed to a fairly restricted set of
options - results of downloads from the database and transformations.

```r
test_that(“as.sequence works”;{
 expect_identical(as.sequence(c(“a”, “b”)), 1:2)
 expect_identical(as.sequence(c(“a”, NA)), 1:2)
 expect_equal(as.sequence(c(“a”, NA), na.rm = FALSE), c(1,NA))
 expect_equal(as.sequence(c(NA,NA)), c(1,1))
})
```

