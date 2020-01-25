
<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/rstudio/bootstraplib.svg?branch=master)](https://travis-ci.org/rstudio/bootstraplib)
[![CRAN
status](https://www.r-pkg.org/badges/version/shinymeta)](https://cran.r-project.org/package=shinymeta)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
<!-- badges: end -->

# bootstraplib

Tools for theming [**shiny**](https://shiny.rstudio.com) and [**rmarkdown**](https://rmarkdown.rstudio.com) from R via [Bootstrap](https://getbootstrap.com/) and [Sass](https://sass-lang.com).

## Installation

**bootstraplib** isn’t yet available from CRAN, but you can install
with:

``` r
remotes::install_github("rstudio/bootstraplib")
library(bootstraplib)
```

## Introduction

The **bootstraplib** R package provides tools for compiling and working
with [Bootstrap
Sass](https://getbootstrap.com/docs/4.4/getting-started/theming/),
making it possible to style **shiny** apps and **rmarkdown** documents
directly from R (via **sass**) instead of writing raw CSS and HTML.
Currently, **bootstraplib** supports Bootstrap 3 and 4, as well as a
special `"4-3"` compatibility version (read more about this in [choosing
a version](#choosing-a-version)). Using **bootstraplib** in **shiny**
and **rmarkdown** is still considered experimental at this point, but
see the notes below to start using it today.

### Shiny

To start using **bootstraplib** in your **shiny** apps today, you'll also need to install an updated version of Shiny (`remotes::install_github("rstudio/shiny")`). Then do the following:

1)  Call `bs_theme_new()` and optionally specify a Bootstrap `version`
    and [`bootswatch` theme](https://bootswatch.com/). The current
    default is Bootstrap 4 (with added added Bootstrap 3 compatibility)
    and no `bootswatch` theme:

<!-- end list -->

``` r
bs_theme_new(version = "4-3", bootswatch = NULL)
```

2)  Once a (global) theme is initialized, you may start adding theming
    customizations; for example [overriding variable
    defaults](https://getbootstrap.com/docs/4.4/getting-started/theming/#variable-defaults),
    and more (see the [theming foundations](foundations.html) article).

<!-- end list -->

``` r
bs_theme_add_variables(
  "body-bg" = "salmon",
  "body-color" = "white"
)
```

3)  Add `bootstrap()` to your user interface (this step might not be
    required in a future version of **shiny**).

<!-- end list -->

``` r
library(shiny)
fluidPage(
  bootstrap(),
  titlePanel("Hello world!")
)
```

<img src="https://i.imgur.com/3wcFKFs.png" width="50%" style="display: block; margin: auto;" />

### R Markdown

To start using **bootstraplib** in your `rmarkdown::html_document`s,
install `remotes::install_github("rstudio/rmarkdown#1706")`, then do the
following:

1)  Use `bootstrap_version` and `theme` to choose the Bootstrap version
    and a Bootswatch theme. These arguments are currently supported only
    by `html_document` and `html_document_base`.

<!-- end list -->

``` yaml
---
output:
  html_document:
    bootstrap_version: 4-3
    theme: minty
---
```

2)  Optionally, add theme customizations inside any R code chunk (these
    customizations end up influencing the Bootstrap CSS included in the
    output document).
    
    ``` {r echo=FALSE}
    library(bootstraplib)
    bs_theme_add_variables(primary = 'salmon')
    ```

## Choosing a version

The **bootstraplib** package currently supports three different
`version`s: `"4-3"`, `4`, and `3`. In the future, when Bootstrap
[releases more major versions](https://github.com/twbs/release),
**bootstraplib** may add more versions, and may also change the default
`version`. However, the default `version` (currently `"4-3"`) will
always be designed to work well with the core **shiny** UI functionality
(e.g., `actionButton()`, `navlistPanel()`, etc). If your UI wants to
assume a specific version of Bootstrap (i.e., it uses a package like
**bs4Dash** or **yonder** to generate UI), then it’s a good idea to set
an explicit `version` (this way, when a new version of Bootstrap is
released, and the default `version` changes, your app won’t break).

Be aware that Bootstrap 4 and 3 expose a very different set of theme
customization entry points, and as a result, theme customizations that
you write for Bootstrap 4 may not necessarily work for Bootstrap 3 (and
vice versa). At the moment, our priority is to enable and improve the
Bootstrap 4 theming experience. If you’re not interested in upgrading to
Bootstrap 4, and would rather theme your Bootstrap 3 project today, you
may want to consider [using **fresh**](#fresh) in the near term.

## Interactive theming

**bootstraplib** also comes with functionality for interactive theming
**shiny** apps (or **rmarkdown** documents with `runtime: shiny`).
Either point `run_with_themer()` to an application directory or use
`bs_theme_preview()` to use a pre-packaged application designed for
theming. Note that as you interactively theme your application, code is
printed to the R console that you can copy/paste to adopt those changes
in your theming code.

``` r
bs_theme_new(bootswatch = "sketchy")
bs_theme_preview()
```

<img src="https://i.imgur.com/il6nd8J.gif" width="80%" style="display: block; margin: auto;" />

## Similar work

The [**fresh** package](https://github.com/dreamRs/fresh) offers an
alternative (and currently more user friendly) approach to theming via
Bootstrap 3 Sass variables. At the moment, **bootstraplib** is more
focused on laying an extensible foundation for theming with Bootstrap 3
(or 4) that other R packages can build upon.

<!--
* Use [Bootstrap](https://sass-lang.com/) 4, as well as any major version of Bootstrap built on [Sass](https://sass-lang.com/). 

  * Packages like **shiny**, **rmarkdown**, and many of their downstream dependencies have long depended on static Bootstrap 3 CSS, and may continue to do so by default. This package provides a means for upgrading to Bootstrap 4 (as well as compiling [Bootstrap 3 SASS](https://github.com/twbs/bootstrap-sass), and potentially, [future major versions](https://github.com/twbs/release) of Bootstrap). 

* Easily use pre-packaged [Bootswatch](https://bootswatch.com/) themes. 

* Create [custom Bootstrap themes](https://getbootstrap.com/docs/4.0/getting-started/theming) through compilation of user supplied [Sass](https://Sass-lang.com) code from R. 

To learn more, see the article on using **bootstraplib** with [**shiny**](articles/shiny.html) and [**rmarkdown**](articles/rmarkdown.html), as well as creating [custom Bootstrap themes](articles/custom.html).
-->
