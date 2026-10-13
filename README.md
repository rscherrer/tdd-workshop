# Test-driven development for ecology and evolution

Welcome to this tutorial on how to implement unit tests in your code! 

This workshop is tailored to ecologists and evolutionary biologists, and was designed for the 2026 [SORTEE](https://sortee.org) conference, with the hope of popularizing this set of good programming practices among scientists who do not necessarily have a computer science or software development background.

The workshop consists of a presentation (slides available in this repository) and a practical. The practical can be done in [R](https://r-project.org) or [Python](https://python.org) and consists of a series of exercises requiring tweaking some code that is made available to the attendees in this repo. The presentation should ideally have been followed before attempting the practical.

This repository contains the code for the R version of the workshop. For the Python version, please use [this](https://github.com/rscherrer/tdd-workshop-python) repository. **Note** that the instructions for the exercises are the same in R and Python, and so they are only shown here.

## Prerequisites

For this practical you will need:

* R (4.3.3+)

as well as the CRAN packages:

* [testthat](https://testthat.r-lib.org/) (3.3.2+)
* [covr](https://covr.r-lib.org/) (3.6.4+)

and their dependencies (which should be automatically downloaded upon installation).

Additionally, the [RStudio](https://posit.co/products/open-source/rstudio) (2024.12.1+) IDE would be very helpful.

## Setup

Please download the repository onto your computer. In it, you will find a series of subfolders. Each subfolder is a slightly more complicated version of the previous one, with complexity increasing as we progress through the exercises in the practical. For each exercise, set the corresponding subfolder as your working directory (for R users, clicking on the `.Rproj` file inside the folder will open RStudio directly in the right directory). Completing the exercise will involve you modifying and/or augmenting the code that is contained in the folder.

The folder contains a code base of tools for population genetics:

* the functions of the toolkit, as scripts inside the `functions/` folder
* a test script (`tests.R/py`) implementing the unit tests to perform on the functions
* some numbered scripts to try the functions, run the tests and measure code coverage

(Other files or folders that may be there are for configuration purposes, please do not edit them.)

As part of the exercises, **you should edit the functions in the `functions/` folder and/or the tests in `tests.R/py` files**. The numbered scripts are the ones which actually launch stuff (tests, code coverage) and therefore they are the ones you would need to run (e.g. rerunning all the tests after having fixed a bug in a function). They can be modified if indicated, but in general avoid touching them too much as they are here to help you. (Of course, nothing prevents you from running any snippet of code you like to see how a function behaves.)

The practical goes through a series of steps to build some reusable code as part of a fictional population genetics small project. The modus operandi is as follows: you have code written, which may contain bugs, and you need to write and run unit tests to figure out where the bugs are and fix them. Each step of the way, we add tests and tweak the code, with two aims in mind: **all tests passing**, and **100% code coverage**.

In summary:

* edit the `functions/` or `tests.R/py` as needed
* run `01_try_functions.R/py` to play around with the toolkit
* run `02_test_functions.R/py` to run all the tests
* run `03_check_coverage.R/py` to measure code coverage

If you get frustrated with an exercise, feel free to check out the solution! In addition, the starting folder for each exercise should have the problems of the previous exercises solved, so you always get a fresh start.

Good luck!

## Instructions

### Exercise 1

You've inherited a bunch of code from a collaborator with whom you want to create a simple toolbox for population genetics. The collaborator tells you that the code was in fact written by their student, for the purpose of a Master's thesis, which was duly completed, with beautiful figures and all. The collaborator even checked that all the code runs without returning an error. You pass the code down to your own student, who is writing a simulation that requires all these pop-gen tools. But your student gets back to you saying that one function in particular, the one calculating the _inbreeding coefficient_ (see Box 1) from a series of genotypes, produces weird results. You pass the code on to your department's scientific programmer, who returns it to you with a bunch of tests written in a separate script. They say that some tests are failing, so there must be a bug (or more?) in the code. We have provided you with the function and the tests in the accompanying code. 

**Your task:**

Run the tests, and see for yourself whether the function behaves as expected. Figure out which tests fail, and see if that helps you find the bugs and fix them. Keep at it until all the tests pass. [Hint: There are three hidden bugs in the inbreeding coefficient function that cause the provided tests to fail.]

**Note for R users:** Here we use the `expect_*` family of testing functions from the [testthat](https://testthat.r-lib.org/) package (see `tests.R`). Useful examples of such functions include:
* `expect_equal()`
* `expect_true()`
* `expect_error()`

**Note for Python users:** The tests are written using the [pytest](https://docs.pytest.org/en/stable/) framework (see `tests.py`). Useful examples of such functions include:
* `assert ... == ...`
* `assert ... is True`
* `with pytest.raises(...): ...`

If whatever expression being evaluated is as expected, these commands should not return anything or say that the tests pass. Otherwise, they will complain. We want to make sure all the tests pass!

Box 1. Inbreeding coefficient (Fis)
|-|
In population genetics, the inbreeding coefficient (Fis) of a population at a given locus (or gene) is the reduction in heterozygosity at that locus which could be attributed to non-random mating within that group or population. It is commonly calculated as:
Fis = (He - Ho) / He
where He is the heterozygosity expected under random mating, and Ho is the observed heterozygosity (i.e. the actual proportion of heterozygotes). The expected heterozygosity He is itself calculated from the frequencies of the different alleles in the population:
He = 1 - p^2 - q^2 - ... (in a diploid population)
where p, q, ..., are the frequencies of each allele (e.g. p = f(A), q = f(a), ...).
The inbreeding coefficient ranges between -1 and 1. The logic behind it is that the more inbreeding, i.e. the more individuals mate with related individuals with similar genetic makeup, the more homozygotes will be produced and the lower the number of heterozygotes in the population (so the higher the coefficient). Zero represents the null expectation under random mating. Basically, inbreeding erodes genetic diversity, and the inbreeding coefficient is a way to measure that.

[Answer](answers/answer_1.md)

### Exercise 2

Whoops, you receive an email from the programmer who tells you that they forgot to test the edge situation in which the entire population consists of a single homozygous genotype (a maximally inbred population). They send you the missing tests, which we have added to the code base.

**Your task:**

Run the tests. Do these new tests pass? If not, what's going on? And can you fix it? [Hint: The programmer assumed that such a population should have an inbreeding coefficient of one. But is that correct, based on the formula for the inbreeding coefficient?]

[Answer](answers/answer_2.md)

### Exercise 3

Now that the bugs are fixed and all the tests pass, it is time to check code coverage, for good measure. We provide you with a third numbered script to do that. If the code is fully tested, code coverage should be 100%.

**Your task:**

Run the script that measures code coverage. Is it 100%? If not, try to figure which lines of code have not been tested. Write tests to cover all edge cases and bring code coverage to no less than 100%. Do all the added tests pass? If not, which one breaks? Can you figure out why and fix any issue there might be? [Hint: Could there be a hidden bug we had not found because we had not covered all the possible paths our function could take??]

**Note for R users:** Here we use functionalities from the [covr](https://covr.r-lib.org/) package. You can encapsulate the output of a `*_coverage` function inside of covr's `report()` function to interactively visualize which lines of code are covered (in green) and which are not (in red), e.g. by running:

```r
report(function_coverage(...))
```

**Note for Python users:** We measure code coverage using the [pytest-cov](https://pypi.org/project/pytest-cov/) plugin. Upon running the script that performs this measurement, you should see a report indicating the percentage of code covered by the tests. An HTML page will also be generated, which you can open in a browser to visually inspect which lines are covered and which are not.

[Answer](answers/answer_3.md)

### Exercise 4

You realize that there is another edge case which you have not covered: that of empty genotypes (e.g. `""` instead of `"Aa"`). Ideally the function should error when provided with an empty genotype. We have added for you a line of code in the input checks to make sure that empty genotypes trigger an error, and a corresponding new test for this new functionality.

**Your task:**

Run the tests again. Do all the tests pass? What about code coverage? What's going on here? Can you fix it? [Hint: Use the interactive coverage report to figure which lines of code might not be run.] Bonus question: When checking for empty genotypes, why are we only looking at the first genotype in the population?

[Answer](answers/answer_4.md)

### Exercise 5

(Advanced.) Having a function that calculates the inbreeding coefficient is great, but a toolbox with just one tool is not exactly groundbreaking. You decide to add to it a function to compute the _fixation index_, or Fst statistic, from two vectors of genotypes.

**Your task:**

Try to build your new function using the test-driven development philosophy: write test, test fails, update code, test passes, repeat. Make sure that your function computes the statistic correctly, handles edges cases and errors appropriately, and that all lines of code are tested. For this question, we have prepared skeleton code for you to complete. 

To help you and avoid code repetition, we have outsourced the initial checks (those performed in the preamble of the inbreeding coefficient function) to a separate function, which is then reused in the preamble of the fixation index function. This now means that the checking function can be tested on its own, separately from its occurrence within the inbreeding coefficient function or the fixation index function. On your way to adding the fixation index function to the code base, move the tests around accordingly, still striving for 100% code coverage and all tests passing.

Box 2. Fixation index (Fst)
|-|
The fixation index (Fst) is a popular measure of the genetic differentiation between two populations at a given locus. It is calculated as a proportion of heterozygotes across two populations taken separately, compared against the null expectation in which both populations are actually one and the same, randomly mixing population. One formula for Fst is:
Fst = (Ht - Hs) / Ht
where Ht is the total heterozygosity that would be expected if both populations were one and the same, and Hs is the average heterozygosity expected across both populations taken separately. In turn, total heterozygosity is computed as:
Ht = 1 - p^2 - q^2 - ... (in a diploid population)
where p, q, ..., are the frequencies of each allele pooled across both populations. In contrast, the average within-population heterozygosity is given as:
Hs = (Hs1 + Hs2) / 2
where 
Hsi = 1 - pi^2 - qi^2 - ... (in a diploid population)
where pi, qi, ..., are the frequencies of each allele within each population i.
The fixation index ranges between -1 and 1. The higher its value, the more the two populations are genetically differentiated, i.e. they each contain an excess of a different allele at the focal locus, such that it looks like the two populations are indeed separate, with their own allele frequencies. A value of zero is expected under the null scenario of no difference in allele frequencies. 

[Answer](answers/answer_5.md)

## Links

* [tdd-workshop](https://github.com/rscherrer/tdd-workshop)
* [tdd-workshop-python](https://github.com/rscherrer/tdd-workshop-python)

## Permissions

Copyright (c) Raphaël Scherrer, 2026.

The code in this repository is shared under the [MIT license](LICENSE.txt) license. The creative content (e.g. slides) is shared under the [CC-BY 4.0](LICENSE_DATA.txt) license. 
