# Questions, Bugs, and Discussions

This repository is intended for getting help and suggestions related to the [r-universe](https://r-universe.dev) project:

 - [issues](https://github.com/r-universe-org/bugs/issues) 
 - [discussions](https://github.com/r-universe-org/bugs/discussions)
 - [project homepage](https://ropensci.org/r-universe/)

Do note that R-universe is a still in early development, with limited human capacity.

Some frequently asked questions are answered below!

## What is r-universe?

The r-universe platform is a new umbrella project by rOpenSci under which we experiment with various new ideas for improving publication and discovery of research software and other research material in R. The system consists of several components and subprojects, that come together on the front-end dashboard on https://r-universe.dev.

For more information see the rOpenSci website: https://ropensci.org/r-universe/


## How to install R packages from r-universe?

You can install packages from r-universe simply using the `install.packages()` command. Because a package may have dependencies that are on CRAN, we also specify the CRAN repository in the repos argument:

```r
# Install 'magick' from 'ropensci' universe
install.packages('magick', repos =
  c('https://ropensci.r-universe.dev', 'https://cloud.r-project.org'))

# Install 'curl' from 'jeroen' universe
install.packages('curl', repos =
  c('https://jeroen.r-universe.dev', 'https://cloud.r-project.org'))
```

Alternatively you can set `options(repos)` to enable favorite repositories by default:

```r
# Enable multiple universes
options(repos = c(
    ropensci = 'https://ropensci.r-universe.dev',
    jeroen = 'https://jeroen.r-universe.dev',
    cran = 'https://cran.r-project.org'))

# Update some packages
install.packages(c("magick", "openssl", "pdftools", "drake"))
```

## How to setup your personal universe

See also this blog post: [How to create your personal CRAN-like repository on R-universe](https://ropensci.org/blog/2021/06/22/setup-runiverse/). In a nutshell:

1. Create a repository called `<username>.r-universe.dev` on the GitHub account for `username`, for example: https://github.com/maelle/maelle.r-universe.dev. The repository must contain a file called [packages.json](https://github.com/maelle/maelle.r-universe.dev/blob/main/packages.json) in the standard format, defining at least the `package` name and git `url` for the packages you want to include.

2. Install the [r-universe app](https://github.com/apps/r-universe/installations/new) on the GitHub account that you want to enable. Choose __enable for all repositories__ when asked.

3. After a few minutes, your source universe will appear on: `https://github.com/r-universe/:username`

4. The universe automatically starts building the packages from your registry. In addition, it will include packages that referenced as Remotes in the description file of one of those packages.

5. The universe automatically syncs and builds your package git repos once per hour.
If you encounter any issues, the actions tab in your source universe may show what is going on, for example: https://github.com/r-universe/maelle/actions



## How to link a maintainer email addresses to a username on r-universe

We use the GitHub username that is associated with a given email address. If the email address that you use in the R package maintainer field, or in commits, is the same as your GitHub account, everything just works. If you use different email addresses, you can easily add additional email addresses in your [GitHub settings](https://github.com/settings/emails).

Once you have validated your email address on GitHub, subsequent builds of R packages will be able to associate the package and contributions to your user account. We collect statistics during the build process, so the changes will not become visible until the next build of each package.

## My profile picture or contributor statistics do not show up

Same as above. If no picture shows up, you probably need to register the email address that you use as R package maintainer in your [GitHub settings](https://github.com/settings/emails), and wait for the package to be rebuilt.


## How to get a package logo shown in the packages tab

We use the same conventions as pkgdown to find a logo for the package. It either needs to be in one of the locations checked by `pkgdown:::find_logo()` (`man/figures/logo.png` and `man/figures/logo.svg`) or set as an opengraph image in your `_pkgdown.yml`.


## How to add custom packages to r-universe

See this blog post: [How to create your personal CRAN-like repository on R-universe](https://ropensci.org/blog/2021/06/22/setup-runiverse/)

## How to prevent a CRAN/BioC package from automatically being indexed by r-universe

CRAN and BioConductor packages with a valid Git URL in the description are automatically indexed by r-universe. To prevent this, add a line to the package DESCRIPTION file with: `Config/runiverse/noindex: true` (Config/ field names are allowed on CRAN). The r-universe scraper runs once per day, so it can take up to 24 hours for the package to be removed from r-universe.

## Is it possible to customize the package build process with custom options/tools/variables

No, that is not possible. Think of R-universe more like your own mini-CRAN. The build environment is actually very similar to that of CRAN, if the package can build on CRAN, it will probably work on r-universe.

This is on purpose: we want to make sure that the packages deployed on r-universe are reproducible and will work on the user machine as well, and do not depend on some hidden settings or tools.

## How to publish vignettes/articles which require custom software/tokens to render

The best solution is to precompute these vignettes on your local machine, see: [How to precompute package vignettes or pkgdown articles](https://ropensci.org/blog/2019/12/08/precompute-vignettes/)


## Are packages on r-universe required to pass CMD check or meet other criteria

No. R-universe is an open publishing system. The system just builds and deploys R packages from git into personal cran-like repositories. The owner of the universe is responsible for their own policies and quality control.

## Does r-universe archive old versions of packages? How does it work with renv?

R-universe does not archive old versions of packages, but we have something better: we track the upstream git url and commit-id in the R package description. This allows renv or similar systems to restore packages in environments that were installed from r-universe. For more details, see this technote: [How renv restores packages from r-universe for reproducibility or production](https://ropensci.org/blog/2022/01/06/runiverse-renv/) 


## How does r-universe analyze system dependencies (C/C++)

After the package has been installed, we use maketools to show external libraries that the package links to:

```r
maketools::package_sysdeps("sf")
##                 shlib      package     headers source              version
## 1   libproj.so.15.3.1    libproj15 libproj-dev   proj              6.3.1-1
## 2   libgdal.so.26.0.4    libgdal26 libgdal-dev   gdal   3.0.4+dfsg-1build3
## 3 libgeos_c.so.1.13.1 libgeos-c1v5 libgeos-dev   geos        3.8.0-1build1
## 4 libstdc++.so.6.0.28   libstdc++6        <NA>    gcc 10-20200411-0ubuntu1
```

In a nutshell, maketools calls `ldd` on the installed R package to see which `.so` files are linked (the 1st column above). Then it uses the distro package manager (e.g. `dpkg`) to query the runtime package deb that provides this file (the 2nd column), and the corresponding headers and source packages. So to emphasize, this does __not__ use heuristics / guessing based on the package description.

The maketools vignette explains this in more detail: [Automatically determine run-time dependencies for R packages on Linux](https://cran.r-project.org/web/packages/maketools/vignettes/sysdeps.html)

## How to add keyword labels to an R package?

If the R package is hosted on GitHub you can add keywords by configuring [repository topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics). Alternatively, you can specify keywords in the `X-schema.org-keywords` field in your package `DESCRIPTION` file. This field [is permitted on CRAN](https://cs.github.com/?q=org%3Acran+X-schema.org-keywords) as well.

The build system may also add some keywords automatically based on analysis of the package, including the names of system libraries that the package links to, or language tags like `c++` or `openmp`.

## How are the dependencies / dependents calculated?

Both are calculated using transitive (recursive) strong dependencies, hence this includes both direct and indirect dependencies. Or in practical terms: other packages that need to be installed at minimum to be able to use the target package.

At package build time, we use `tools::package_dependencies(pkg, recursive = TRUE)` to derive the list of hard dependencies and store this in the database. R-base packages are not included. The `dependencies` value is simply the size of this list. The `dependents` value is the opposite: this is the number of packages in R-universe which list the target package as one of its dependencies.

## How is the package-rank score calculated?

The exact algorith is very much WIP and will change. Right now it is a combination of:

 - Number of dependents (aka recursive reverse depenencies)
 - Number of Github stars
 - Number of unique contributors
 - Commit activity over the last year

The score is a weighted sum of the above, on a log scale. But we plan to improve the algorithm as the project gets more mature, and better data becomes available.

## Why is a package sometimes listed twice?

If a source package fails to build (which means something is very wrong) then you see a red “build failure” message.
If there was a previous successful build, it is kept there as well for users to install.

## Does R-universe have Linux binaries?

Yes, but right now we only provide binaries for a single linux target: `r-release` on `ubuntu:latest`. This is the platform that the r-universe build system itself runs via the [base-image](https://github.com/r-universe-org/base-image), and we use these binaries when installing dependencies of a package.


To use these binaries on Linux you need to add the corresponding repository to your `repos`. It is not needed to set a custom UserAgent. For example for the `jeroen` universe:

```r
options(repos = c(
  linux = 'https://jeroen.r-universe.dev/bin/linux/jammy/4.3/',
  sources = 'https://jeroen.r-universe.dev',
  cran = 'https://cloud.r-project.org'
))
```

See how [this is done automatically in the base image](https://github.com/r-universe-org/base-image/blob/f20ec9fc6f51ef8a89aad489206a43790bd9bf77/Rprofile#L10-L16) when an environment variable `MY_UNIVERSE` is set.

## How to use WebAssembly Binaries

We have experimental support for webassembly binaries! You should use the official [r-wasm repository](https://repo.r-wasm.org) for dependencies. For example to install the dev version of dplyr:

```r
install.packages('dplyr',
  repos = c('https://tidyverse.r-universe.dev', 'https://repo.r-wasm.org'))
```

You can test them in the [webr demo page](https://webr.r-wasm.org/latest/). Note that this is experimental and not all R packages can be built for wasm.

## Which packages get included in search?

In r-universe, a given R package can be included in many different registries. To prevent duplicates, search only shows packages from the universes of the upstream github owners of the package, and not from 3rd parties that have added a copy of the package to their registry.

More specifically, search results include packages for which:

 - The github owner is the same as the universe, for example packages under `https://jeroen.r-universe.dev` from GitHub user `https://github.com/jeroen/`. This also includes all CRAN packages.
 - Alternatively: to opt-in packages for which the universe name is different from the GitHub owner, the package author must include the full `user.r-universe.dev` in the URL field of the DESCRIPTION file. For example, if the registry from `https://jeroen.r-universe.dev` has a package `https://github.com/elsewhere/mypkg`, but the URL field in the DESCRIPTION includes e.g. `https://jeroen.r-universe.dev` or `https://jeroen.r-universe.dev/mypkg`, then it also shows up in the search results.

## How to link to your r-universe on common websites

**Hugo Academic Theme**: To display the rocket icon hyperlinked to your r-universe in your social icons below your photo add the following code to the social section of *content/authors/admin/_index.md* (or the *_index.md* file for the relevant author), replacing `username` with the relevant user or organisation name.

```md
social:
- icon: rocket
  icon_pack: fa
  link: https://username.r-universe.dev/
```
