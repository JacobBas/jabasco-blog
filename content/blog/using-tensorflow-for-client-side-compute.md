---
title: "Using Tensorflow.js for Client Side Compute"
date: 2021-09-30T21:21:11-07:00
draft: false
desc: "In cases where you want as little computation to be done on the server, Tensorflow.js can be used as a great medium for client-side compute."
---

<div class="blog-post">

## Intro { .display .display-3 .text-italic }
At my current job, one of the largest problems that we face is the problem of limited compute
power for our analytical web applications or the server's ability to handle large computational 
loads. This causes all sorts of problems such as slow compute times, server outages, and 
unintended effects of slowing down the workflows of peers.

The easiest solution to this is to push the computational load from the server onto the client
where the server can act as more of a transport mechanism for the data. This solution has one 
very obvious assumption to it and that's that all users will have similarly powerful machines
which is an appropriate assumption for the internal tooling that I'm creating.

With pushing these processes to the client and to the browser, most languages are out of the
question without resorting to WebAssembly, so JavaScript is the defacto place to look where
Tensorflow.js is the best tool for the job. 

<br />

## Tensorflow.js { .display .display-3 .text-italic }
Tensorflow is best described on its [website](https://www.tensorflow.org/js) as 
"an open-source end-to-end platform for creatingMachine Learning applications" where the 
.js at the end means that it is the JavaScript implementation of this tool.

To add Tensorflow.js to your project it is a simple as adding this cdn link to the top
of an HTML file:
```HTML
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@3.9.0/dist/tf.min.js"></script>
```

While there are a ton of [awesome and complex things](https://www.tensorflow.org/js/demos) 
that you can do with Tensorflow.js we are going to stick to solving systems of equations 
using its built-in gradient descent algorithms.


###  Gradient Descent { .display .display-4 .text-italic }
To keep this post short, I'm not going to go into the nuances of gradient descent, but at a
very high-level gradient descent is an iterative process of solving for systems of equations
by finding solutions that minimize a specific error term. The best-case scenario is a solution 
where the error term is equal to zero, but in most cases with real-world data that is not
possible.

Wikipedia has a great introduction if you'd like to learn more: 
[https://en.wikipedia.org/wiki/Gradient_descent](https://en.wikipedia.org/wiki/Gradient_descent)

<br />

## Solving a System of Equations { .display .display-3 .text-italic }
When talking about complex modeling and machine learning techniques it really all boils down
to solving for systems of equations given a specific set of data or observations with a 
variable number of unknowns which is why it's so important to be able to solve for the 
simple case.


### Simple Example { .display .display-4 .text-italic }
For this example, we are going to be solving for the simple system of equations below:

```latex 
24x   + 5y  - 10z = 100
-100x + 0y  + 12z = 57
99x   - 92y + 12z = 88
```

To start, we need to define some important objects for solving this system: 

```javascript
// initialize the tensors containing the known values
let a = tf.tensor([24, -100, 99], [3, 1]);
let b = tf.tensor([5, 0, 92], [3, 1]);
let c = tf.tensor([10, 12, 12], [3, 1]);
let d = tf.tensor([100, 57, 88], [3, 1]);

// initialize the unknown varibles to solve for
let x = tf.variable(tf.tensor([getRandom(1)]));
let y = tf.variable(tf.tensor([getRandom(-1)]));
let z = tf.variable(tf.tensor([getRandom(1)]));

// create the function from the tensors and unknowns
const func = (a, b, c) => {
    return a.mul(x).add(b.mul(y)).add(c.mul(z));
};

// define our error function to optimize
const mae = (d, d_hat) => {
    return d_hat.sub(d).square().mean();
};
```

Once we have initialized these objects, we can run the gradient descent using the selected
optimizer:

```javascript
// initializing the learn rate and optimization functions
const learnRate = 0.1;
const optimizer = tf.train.adam(learnRate);

// running the gradient descent on the system of equations
for (let i = 0; i < i + 2; i++) {
    // initializing our error term and significance
    let error_rounded;
    let sigfigs = 10000;

    // running the optimizer for our variables
    optimizer.minimize(() => {
        error = mae(d, func(a, b, c));
        error_rounded = Math.round(error.dataSync()[0] * sigfigs) / sigfigs;
        return error;
    });

    // logging iteration milestones
    if (i % 100 === 0) {
        console.log(`iterations: ${i}, error: ${error_rounded}`);
    }

    // breaking if our error is zero to a specific significance
    // or the iteration count is 5000 iterations
    if (error_rounded === 0 || !error_rounded || i === 5000) {
        let final_message = ``;
        final_message += `iterations: ${i} \n`;
        final_message += `error term: ${error_rounded} \n`;
        final_message += `formula: ${x.dataSync()}*a + ${y.dataSync()}*b + ${z.dataSync()}*c = d`;
        console.log(final_message);
        break;
    }
}
```

Click the button below to watch this run in real time!

<div>
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@3.9.0/dist/tf.min.js"></script>

<style>
    .results-button {
        padding: .25rem .75rem;
        text-transform: uppercase;
        background-color: var(--gray3);
        border: 1px solid var(--gray2);
        box-shadow: -2.5px -2px 0px 0px var(--gray2);
        border-radius: 4px;
        font-weight: 600;
        font-style: italic;
    }

    #simple-results-table {
        border-collapse: collapse;
        box-shadow: -2.5px -2px 0px 0px var(--gray2);
    }

    #simple-results-table td,
    #simple-results-table th {
        width: clamp(8em, 10vh, 12em);
        text-align: center;
        border: 1px solid var(--gray2);
    }

    #simple-results-table th {
        background-color: var(--gray3);
    }
</style>


<button class="results-button" onclick="SolveSystem()">Solve System</button>

<table id="simple-results-table">
    <thead>
    <tr>
        <th>Iteration</th>
        <th>MSE</th>
        <th>x</th>
        <th>y</th>
        <th>z</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td id="table-simple-iteration">0</td>
        <td id="table-simple-mse">0</td>
        <td id="table-simple-x">1</td>
        <td id="table-simple-y">-1</td>
        <td id="table-simple-z">1</td>
    </tr>
    </tbody>
</table>

<script>
    var isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);
    console.log(isMobile)

    // selecting the dom elements that we want to update
    let table_simple_iteration = document.getElementById(
    "table-simple-iteration"
    );
    let table_simple_mse = document.getElementById("table-simple-mse");
    let table_simple_x = document.getElementById("table-simple-x");
    let table_simple_y = document.getElementById("table-simple-y");
    let table_simple_z = document.getElementById("table-simple-z");

    // triggers solving the system of equations
    function SolveSystem() {
        new Promise((resolve) => {
            // initialize the tensors containing the known values
            let a = tf.tensor([24, -100, 99], [3, 1]);
            let b = tf.tensor([5, 0, 92], [3, 1]);
            let c = tf.tensor([10, 12, 12], [3, 1]);
            let d = tf.tensor([100, 57, 88], [3, 1]);

            // initialize the unknown varibles to solve for
            let x = tf.variable(tf.tensor([1]));
            let y = tf.variable(tf.tensor([-1]));
            let z = tf.variable(tf.tensor([1]));

            // create the function from the tensors and unknowns
            const func = (a, b, c) => {
                return a.mul(x).add(b.mul(y)).add(c.mul(z));
            };

            // define our error function to optimize
            const mse = (d, d_hat) => {
                return d_hat.sub(d).square().mean();
            };

            // setting the learning rate and the optimization function
            // we are going with the adam optimization function since it
            // adaptively updates the learning rate which leads to slower
            // iteration time, but better performance in the long
            // run when we are trying to estimate the parameters; by using
            // adam, we have a much better and more consitent time converging
            // to the true values of the varibles
            const learnRate = 0.1;
            const optimizer = tf.train.adam(learnRate);

            let i = 0;
            var intr = setInterval(() => {
            // initializing our error term and significance
            let error_rounded;
            let sigfigs = 10000;

            // running the optimizer for our variables
            optimizer.minimize(() => {
                error = mse(d, func(a, b, c));
                error_rounded =
                Math.round(error.dataSync()[0] * sigfigs) / sigfigs;
                return error;
            });

            // setting the displayed values
            table_simple_iteration.innerText = i;
            table_simple_mse.innerText = error_rounded;
            table_simple_x.innerText =
                Math.round(x.dataSync()[0] * sigfigs) / sigfigs;
            table_simple_y.innerText =
                Math.round(y.dataSync()[0] * sigfigs) / sigfigs;
            table_simple_z.innerText =
                Math.round(z.dataSync()[0] * sigfigs) / sigfigs;

            // breaking if our error is zero to a specific significance
            // or the iteration count is 5000 iterations
            if (error_rounded === 0 || !error_rounded || i === 2000) {
                clearInterval(intr);
            }

            // adding one to the iteration
            ++i;
            }, 0);

            // resolving the promise
            resolve();
        });
    }
</script>
</div>

<br />

## Final Thoughts { .display .display-3 .text-italic }
With a little bit of extra work, Tensorflow.js can be used to solve even more complex 
systems of equations with a variable number of unknowns that are defined by the user.
Models can even be created on the server using Python or Node.js and moved over to 
the client once they are finalized. 

This really opens the door to the possibilities of moving away from server-side compute
where it is not needed and can allow for a more user-friendly experience when analyzing
and predicting values from data.

</div>

