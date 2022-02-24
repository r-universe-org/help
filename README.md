# Bugs, Support and Discussions

## Resources

This repository is intended for getting help and suggestions related to the [r-universe](https://r-universe.dev) project:

 - [issues](https://github.com/r-universe-org/bugs/issues) 
 - [discussions](https://github.com/r-universe-org/bugs/discussions)

We will try to address issues, but do note that R-universe is a still in early development, with limited human capacity, so please be patient.

## What is r-universe

The r-universe platform is a new umbrella project by rOpenSci under which we experiment with various new ideas for improving publication and discovery of research software and other a in R. The system consists of several components and subprojects, that come together on the front-end dashboard on https://r-universe.dev. For more information see the rOpenSci website: https://ropensci.org/r-universe/


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

Alternatively you can set `options(repos)` to enable favoriate repositories by default:

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

To setup a universe for your own GitHub user or organization:

1. Create a repository called `universe` on the GitHub account for which you want to create a universe. The repository must contain a file called [packages.json](https://github.com/r-universe-org/demo-registry/blob/master/packages.json) in the standard format, defining the package and git url for the packages you want to include. For example: https://github.com/maelle/universe. Start by adding no more than a few packages, you can add more later.

2. Install the [r-universe app](https://github.com/apps/r-universe/installations/new) on the GitHub account that you want to enable. Choose __enable for all repositories__ when asked.

3. After a few minutes, your source universe will appear on: `https://github.com/r-universe/:yourname`

4. The universe automatically starts building the packages from your registry. In addition, it will include packages that referenced as Remotes in the description file of one of those packages.

5. The universe automatically syncs and builds your package git repos once per hour.
If you encounter any issues, the actions tab in your source universe may show what is going on, for example: https://github.com/r-universe/maelle/actions



## How to link a maintainer email addresses to a username on r-universe

We use the GitHub username that is associated with a given email address. If the email address that you use in the R package maintainer field, or in commits, is the same as your GitHub account, evertying just works. If you use different email addresses, you can easily add additional email addresses in your [GitHub settings](https://github.com/settings/emails).

Once you have validated your email address on GitHub, subsequent builds of R packages will be able to associate the package and contributions to your user account. We collect statistics during the build process, so the changes will not become visible until the next build of each package.

## My profile picture or contributor statistics do not show up

Same as 1. If no picture shows up, you need to register the email address that you use as R package maintainer in your [GitHub settings](https://github.com/settings/emails), and wait for the package to be rebuilt.


## How to get a package logo shown in the packages tab

We use the same conventions as pkgdown to find a logo for the package. It either needs to be in one of the locations checked by `pkgdown:::find_logo()` or set as an opengraph image in your `_pkgdown.yml`.


## How to add custom packages to r-universe

See this blog post: [How to create your personal CRAN-like repository on R-universe](https://ropensci.org/blog/2021/06/22/setup-runiverse/)


## It is possible to customize the package build process with custom options/tools/variables

No, that is not possible right now. This is on purpose: we want to make sure that the packages deployed on r-universe are reproducible and will work on the user machine as well, and do not depend on some hidden settings or tools.

Think of R-universe more like your own mini-CRAN. The build environment is actually very similar to that of CRAN, if the package can build on CRAN, it will probably work on r-universe.


## How to publish vignettes/articles which require custom software/tokens to render

The best solution is to precompute these vignettes on your local machine, see: [How to precompute package vignettes or pkgdown articles](https://ropensci.org/blog/2019/12/08/precompute-vignettes/)


## Are packages on r-universe required to pass CMD check or meet other criteria

No. R-universe is an open publishing system. The system just builds and deploys R packages from git into personal cran-like repositories. The owner of the universe is responsible for their own policies and quality control.

## Does r-universe archive old versions of packages? How does it work with renv?

R-universe does not archive old versions of packages, but we have something better: we track the upstream git url and commit-id in the R package description. This allows renv or similar systems to restore packages in environments that were installed from r-universe. For more details, see this technote: [How renv restores packages from r-universe for reproducibility or production](https://ropensci.org/blog/2022/01/06/runiverse-renv/) 



