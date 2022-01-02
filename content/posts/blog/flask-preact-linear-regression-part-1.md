---
layout: post
title: "Flask/Preact Linear Regression Application: Part 1 - Introduction"
date: 2021-12-30T14:30:22-07:00
description: In part 1 of this series of posts I go through my reasoning in choosing Flask and Preact.js as the stack when developing a simple analytical web application and why it is a good choice for prototyping projects. 
draft: false
---

## Intro 
One of my favorite tech stacks for prototyping and designing simple applications and tools for use by myself and internally within the companies that I've worked for is Python Flask and Preact.js. It is the perfect marriage of power, extensibility, and simplicity that allows for complex and modular applications that add a ton of benefit given the effort involved.

For an example project that uses this stack please see the example that we will be building out through this series: https://github.com/JacobBas/flask-preact-linear-regression

## Why Use This Stack?
When looking for this type of tech stack the two things that are most important for me are: 
1. Minimal but extensible so that the project can start off simple but has the ability to become greater. 
2. A low start up cost so that I can feel comfortable scrapping the whole idea if it doesn't work out or refactoring code if more stability is needed.

Flask and Preact have both of these attributes on their own so together they work to magnify the power and complexity that can be added with very little cost to the developer. Below I go into a bit more detail as for why this is the case.

### Minimal
I might be biased due to my experiences or lack of experience but I like to use tools that are very minimal when it comes to functionality and dependency. This is even more true when trying to prototype something out to get a sense of the value that it may add. With minimal tools the start up time and speed at which I can develop always seems to be quick since I can focus on the problem at hand rather than the setup and API that I am using.

For Flask, being minimal is at it's very root given it's status as a "microframework". It really aims at being simple to start up and develop off of where the [simple case application](https://flask.palletsprojects.com/en/2.0.x/quickstart/#a-minimal-application) fits within 7 lines of code.

Given the low dependency and easy boiler plate code, getting started up with a project has low cost to the developer.

The same can be said for Preact. Alot of the benefit comes from the fact that it is very minimal in terms of it's dependency and size making it easy to inject into a frontend. I like to think of Preact as the smaller cousin of React that includes all 90% of the functionality you would ever need at a much smaller bundle size.

My favorite use case is to use the Preact by way of CDN with the `htm` module adding in `JSX` like syntax without the use of a build process. Getting it to work is as simple as adding the following 4 lines to the top of a `.js` module:
``` javascript
import * as Preact from "https://cdn.skypack.dev/preact@10.4.7"; // for preact
import * as Hooks from "https://cdn.skypack.dev/preact@10.4.7/hooks"; // for preact hooks
import htm from "https://cdn.skypack.dev/htm@3.0.4"; // for htm
export const html = htm.bind(_Preact.h);
```

### Extensible
While the simplicity of this stack is awesome for starting up a project, the extensibility of both tools are what add alot of benefit as a project develops from prototype to production.

This attribute, which is true for both Flask and Preact, is best stated by the Flask [documentation](https://flask.palletsprojects.com/en/2.0.x/foreword/): 
> "The “micro” in microframework means Flask aims to keep the core simple but extensible. Flask won’t make many decisions for you ... Everything else is up to you, so that Flask can be everything you need and nothing you don’t."

For example, if you need to create a REST API in Flask with Swagger integration and a more comprehensive tooling you could always reach for [Flask-RESTX](https://flask-restx.readthedocs.io/en/latest/) where an admin pannel for a website can easily be integrated through the add-on [Flask-Admin](https://flask-admin.readthedocs.io/en/latest/).

Similarly, Preact comes with great integrations for both React and Preact specific components which opens up your project to the large community of open source resources.

### Easy to Throw Away
Given the low-cost, minimal, and extensible nature of this stack it never feels like a tough decision deleting code or even scrapping an idea entirely. Having that amount of freedom due to the simplicity and modularity of Flask and Preact allows you to be more creative and have a more enjoyable development experience.


## TL;DR
When considering a tech stack for simple projects and prototypes, the first frameworks that I tend to reach for are Flask and Preact due to the simplicity and extensibility that they allow the developer. With a low startup cost and high complexity ceiling most problems can be solved with this stack without worrying about about a poor cost to benefit ratio.
