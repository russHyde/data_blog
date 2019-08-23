---
title: New-Package Checklist
author: Russ Hyde
date: '2019-08-23'
slug: new-package-checklist
categories:
  - R
tags:
  - coding
lastmod: '2019-08-23T14:35:00+01:00'
layout: post
type: post
highlight: no
---

**Things to do when starting a new R package.**

## Before setup

- Name selected using `available::suggest()` and
  `available::available("prospective_pkg", browse = FALSE)` 

- New repository on github (without readme / gitignore / license)

- Ensure all packages that your new package will rely on are available in your
current R environment

## Initial setup

- New project in rstudio

    - New Project --> New Directory --> R Package

    - Define package name to match the github repo

- Delete template files (`R/hello.R`; `man/hello.Rd`)

- Delete `NAMESPACE`

    - having an existing NAMESPACE that wasn't created by `devtools::document()` prevents the latter from updating the `NAMESPACE` file

- Set project defaults in `*.Rproj` - Use existing default plus

    - don't load .RData; don't save .RData

    - Line-Ending Posix (LF)

    - Generate documentation with Roxygen (automatically on Build & Reload)

- Load `devtools`, `usethis`, `testthat`, `lintr`, `styler` and `goodpractice`
into current workspace in Rstudio

- Set your full-name in the options for `usethis`:

    - `options(usethis.full_name = "My Full Name")`

- 'Clean & Rebuild' the package (if there's a warning about a missing line in
`DESCRIPTION`, just delete the final line and save the file, then 'Clean &
Rebuild' again)

## README

- Add a `README.Rmd` file (adds the path to `.Rbuildignore` as well)

    - `usethis::use_readme_rmd()`

    - opened the file and knitted it within Rstudio (generates `README.md` from
    `README.Rmd`)

## DESCRIPTION

- Update the title, description and Author/Maintainer info to `DESCRIPTION`

    - Don't use Authors@R

- Update the license

    - `usethis::use_mit_license()` 

        - `usethis` will get your full name from the options you set earlier (`usethis.full_name`)

  - Your name should have been added to LICENSE (if you set the
  `usethis.full_name` option) otherwise add it manually

- Add `URL` and `BugReports` fields to `DESCRIPTION`

## Version Control

You can now build the package and push the directory to github

- cd into your package from the command line

~~~~
# initial commit
git add .
git commit -m "new package ..."
git remote add origin git@github.com:...
git push -u origin master`
~~~~

## Tooling

### Linting

- (? should there be a `usethis::use_lintr` that adds a basic `.lintr` to
    the file path and to `.Rbuildignore`)

- `.lintr` file is shown below (and is stored in the main directory of the
    package)

- Add `^\.lintr$` to .Rbuildignore

- Note: the `.lintr` file is not visible within the file selector dialogue
    in Rstudio

### Styling

- No modifications need to be made to use `styler`

- this is only used during development, not during continuous-integration
    tests

### Testing 

- `usethis::use_testthat()`

- Note: running `Check` on the package will now fail until some tests are
    written

### Continuous Integration (initialisation)

Add files for Travis continuous integration tools:

- `usethis::use_travis()`

- Sync travis with github (Just click `Sync account` within your travis
    repositories page)

- Reknit the `README.Rmd` to `.md` (this should now have a Travis URL in
    the 'badges' section)

### Code coverage

Add code coverage integration:

- `usethis::use_coverage()`

- Reknit the `README.Rmd` (a coverage badge / URL should have been added)

- Add a test file to `tests/testthat/test_<something>.R`

    - Head this with `context("Tests for ...")`

- Add a source file to `R/<something>.R`

- You need at least one test function before `covr::package_coverage()`
    will run and `covr::codecov()` will run without error on Travis

### Continuous Integration (Configuration and Running) 

- Update the `.travis.yml` file to allow bioconductor repositories (if
required) and to run both `lintr` checking and code coverage on travis (See the
`.travis.yml` below)

- Push to github

- Activate the new repo on your travis repositories

- Trigger a new build on travis 

    - it will automatically build from your github repo on subsequent pull
    requests, but won't automatically build from a newly-synced repository

    - this should fail on travis (unless you've written some testthat tests and
    written some documented functions / classes)

## Source Code

TODO

## Files

My default `.lintr` file:

```
linters: with_defaults(
  commented_code_linter = NULL,
  line_length_linter(80),
  object_length_linter(40),
  open_curly_linter = NULL,
  spaces_left_parentheses_linter = NULL
  )
```

My default `.travis.yml` file:

```
# R for travis: see documentation at https://docs.travis-ci.com/user/languages/r

language: R
sudo: false
cache: packages

r_github_packages:
  - jimhester/lintr

bioc_required: true
use_bioc: true

env:
  - LINTR_COMMENT_BOT=true

after_success:
  - R CMD INSTALL $PKG_TARBALL
  - Rscript -e 'lintr::lint_package()'
  - Rscript -e 'covr::codecov()
```