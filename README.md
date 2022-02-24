# Bugs, Support and Discussions

## Resources

This repository is intended for getting help and suggestions related to the [r-universe](https://r-universe.dev) project:

 - [issues](https://github.com/r-universe-org/bugs/issues) 
 - [discussions](https://github.com/r-universe-org/bugs/discussions)

We will try to address issues, but do note that R-universe is a still in early development, with limited human capacity, so please be patient.


## FAQ

### 1. How to link a maintainer email addresses to a username on r-universe

We use the GitHub username that is associated with a given email address. If the email address that you use in the R package maintainer field, or in commits, is the same as your GitHub account, evertying just works. If you use different email addresses, you can easily add additional email addresses in your [GitHub settings](https://github.com/settings/emails).

Once you have validated your email address on GitHub, subsequent builds of R packages will be able to associate the package and contributions to your user account. We collect statistics during the build process, so the changes will not become visible until the next build of each package.

### 2. My profile picture or contributor statistics do not show up

Same as 1. If no picture shows up, you need to register the email address that you use as R package maintainer in your [GitHub settings](https://github.com/settings/emails), and wait for the package to be rebuilt.


### 3. How to get a package logo shown in the packages tab

We use the same conventions as pkgdown to find a logo for the package. It either needs to be in one of the locations checked by `pkgdown:::find_logo()` or set as an opengraph image in your `_pkgdown.yml`.


### 4. How to add custom packages to r-universe

See this blog post: [How to create your personal CRAN-like repository on R-universe](https://ropensci.org/blog/2021/06/22/setup-runiverse/)


### 5. It is possible to customize the package build process with custom options/tools/variables

No, that is not possible right now. This is on purpose: we want to make sure that the packages deployed on r-universe are reproducible and will work on the user machine as well, and do not depend on some hidden settings or tools.

Think of R-universe more like your own mini-CRAN. The build environment is actually very similar to that of CRAN, if the package can build on CRAN, it will probably work on r-universe.


### 6. How to publish vignettes/articles which require custom software/tokens to render

The best solution is to precompute these vignettes on your local machine, see: [How to precompute package vignettes or pkgdown articles](https://ropensci.org/blog/2019/12/08/precompute-vignettes/)


### 7. Are packages on r-universe required to pass CMD check or meet other criteria

No. R-universe is an open publishing system. The system just builds and deploys R packages from git into personal cran-like repositories. The owner of the universe is responsible for their own policies and quality control.

### 8. Does r-universe archive old versions of packages? How does it work with renv?

R-universe does not archive old versions of packages, but we have something better: we track the upstream git url and commit-id in the R package description. This allows renv or similar systems to restore packages in environments that were installed from r-universe. For more details, see this technote: [How renv restores packages from r-universe for reproducibility or production](https://ropensci.org/blog/2022/01/06/runiverse-renv/) 



