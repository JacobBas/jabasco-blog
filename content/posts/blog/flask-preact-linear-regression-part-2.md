---
layout: post
title: "Flask/Preact Linear Regression Application: Part 2 - Initializing the Project Structure"
date: 2021-12-31T14:30:22-07:00
description: In part 2 of this series of posts I go through initializing the project structure for the analytical web application that we will building throughout the rest of the project.
draft: false
---

## Intro 
In part 2 of this series we will initialize our project structure that will be populated with the actual code. 

Please see the linked GitHub repository for the completed project that we will be building throughout this series: https://github.com/JacobBas/flask-preact-linear-regression


## Initializing the Project
Before starting, I want to make a point in mentioning that when prototyping out a project the code/folder structure is designed in conjunction with writing the code to allow for a more freeform development of the project. In most cases, I will start off with very few scripts and refactor into more specified scripts as the project becomes larger and more complex. This is true for dependencies as well when attempting to find the right tools for the job.

### Creating the Folders and Populating with Scripts
To start the project, we want to create the following folder and files:
```zsh
├── backend
│   ├── __init__.py
│   ├── _api
│   │   ├── __init__.py
│   │   ├── linear_regression.py
│   └── _pages
│       ├── __init__.py
├── frontend
│   ├── index.html
│   └── static
│       ├── main.css
│       └── main.js
├── .gitignore
├── main.py
├── README.md
└── requirements.txt
```

#### Root Folder
In the root of the folder we have a few important items:
- `.gitignore`: used to ignore files or folders within our version control
- `main.py`: main script for running the application
- `README.md`: used for documentation of the application
- `requirements.txt`: contains version information for our python packages 

#### Backend Folder
Within the backend folder we contain all of the important scripts used in our Flask backend:
- `__init__.py`: initializes our folder as a python module
- `_api`: contains the logic for our REST API
- `_pages`: contains the logic for serving any additional pages in our application

#### Frontend Folder
The frontend folder contains all of the important code used within the UI of the application:
- `index.html`: the base HTML template for the UI
- `main.css`: styling for the UI
- `main.js`: contains the Preact components for the application UI


### Adding In the Required Dependencies
Since the main goal of this framework is to be low dependency this piece of set up should be pretty easy.

#### Python
To set up our python dependencies, all we need to do is paste the following text into our `requirements.txt` file:
```python
# for creating the HTTP server
flask==2.0.2
flask-restx==0.5.1

# for calculating a linear regression
scipy==1.7.3

# for formatting code
black==21.12b0
```

To finish our python setup we need to create our venv (virtual environment) to house our dependencies. This can be done by running the following lines of code in the terminal:
1. `python3 -m venv venv`
2. Initialze the venv: 
   - Linux/Unix/MacOS: `source venv/bin/activate` 
   - Windows: `venv\Scripts\Activate.ps1`
3. `pip install -r requirements.txt`

#### JavaScript
For JavaScript we make use of ES Modules and imports by CDN. In the `main.js` file we will want to add the following lines:
```javascript
// IMPORTS
import * as _Preact from "https://cdn.skypack.dev/preact@10.4.7";      // for preact
import * as _Hooks from "https://cdn.skypack.dev/preact@10.4.7/hooks"; // for preact hooks
import htm from "https://cdn.skypack.dev/htm@3.0.4";                   // for htm

// EXPORTS
/**
 * specifying exports so that they can be used within other JavaScript scripts
 * in the application. Without these exports we tend to run into import errors with
 * the Preact library so these are essential when using JS Modules.
 */
export const html = htm.bind(_Preact.h);
export const Preact = _Preact;
export const Hooks = _Hooks;
```

#### HTML
In order to make use of the JavaScript that we are writting we need to add some boilerplate HTML code into the `index.html` file:
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Linear Regression Application</title>

        <!-- main css styling for the application -->
        <link rel="stylesheet" href="/static/main.css" />
    </head>
    <body>
        <!-- root div to render the app -->
        <div id="root" class="container mt-3" style="margin-bottom: 300px"></div>

        <!-- plotly for plotting data -->
        <script src="https://cdn.plot.ly/plotly-2.8.3.min.js"></script>

        <!-- for uuid values -->
        <script src="https://cdnjs.cloudflare.com/ajax/libs/uuid/8.1.0/uuidv4.min.js"></script>

        <!-- main javascript/preact module -->
        <script type="module" src="/static/main.js"></script>
    </body>
</html>
```

### Initializing Version Control
The last thing that we want to do is to initialize some kind of version control. I'm choosing to use Git:
1. `git init .`
2. `gti add .`
3. `git commit -m "initial commit"`
