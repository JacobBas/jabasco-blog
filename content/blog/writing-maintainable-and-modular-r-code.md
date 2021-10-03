---
title: "Writing Maintainable and Modular R Code"
date: 2021-09-29T11:49:27-07:00
draft: false
desc: "The standard library for R does not support modular code so by using the packages box and renv we can support better code management and maintainability."
---

<div class="blog-post">

One of the largest problems that I have with writing R code is that it does not come with standard tooling for creating modular code which contributes to the unmaintainability of larger R projects. Since R code relies heavily on namespaces/environments and, by default, a global namespace it's difficult to create a separation of responsibilities without some extra tooling.

My two favorite packages for supplementing this are:
1. renv: https://rstudio.github.io/renv/articles/renv.html
2. box: https://github.com/klmr/box

My recommendation is to use these two packages with any new R project. 

<br/>

## renv { .display .display-3 .text-italic}
The purpose of `renv` is similar to `venv` in python, modules in go, or cargo in rust; To help with dependency management so projects can be more stable and reproducible.

`renv` does this by containing all work done to a single directory while also keeping a running list of dependencies and their versions. It also comes with a nice set of tooling to help with initializing, hydrating, and editing the packages that are used within a specific project.

<br/>

## box { .display .display-3 .text-italic}
The purpose of `box` is to help make code and the relationship between dependencies and other scripts more explicit by forcing the user to make deliberate use of import and export statements. As with `renv`, `box` points R in the direction of other more standardized languages such as python, go, and rust by supporting strict imports and exports.

<br/>

## Example Project and Setup { .display .display-3 .text-italic}
An example repository for how these tools can be used together can be found <span class="text-800 text-italic">[here](https://github.com/JacobBas/renv-box-example)</span>. 

There are 4 main steps to getting a project like this setup:
1. Initialize the `renv` with `renv::init()`.
2. Activate the `renv` with `renv::activate()`.
3. Install and record the packages with `renv::install("<package>")` and `renv::record("<package>")`.
4. Import packages and modules with `box::use()` at the top of each script.

<br/>

## Further Uses { .display .display-3 .text-italic}
One of the most interesting use cases for these two packages is leveraging them along side R shiny and shiny modules to create applications that are highly modular and extensible.

</div>
