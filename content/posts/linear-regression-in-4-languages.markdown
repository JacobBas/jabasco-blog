---
layout: post
title:  "Least Squares Regression in 4 Languages"
date:   2021-11-01
description: Calculating a least squares linear regression in C/C++, Rust, Go, and Python. 
categories: 
---
## Intro
One thing that I have consistently struggled with in the past year is figuring out which language I should devote my time to. In my current work, I primarily use Python, JavaScript, and R so I've natuaraly built up a proficiency using these languages, but my toolbox is missing a statically typed, compiled language that I can fall back on.

I've been in this loop of attempting to learn C/C++, Rust, and Go but have struggled to develope a deeper understanding of the languages due to the constant context switching. They all have obvious strengths, weaknesses, and niches that they fill, but at the same time they are robust enough to complete most any project.

To try and get at the root of what I enjoy coding in the most, below I've completed a simple least squares linear regression in C/C++, Go, Python, and Rust to attempt to weed out the differences and similarities in the languages.

## Python
Python has been added to this list as a baseline since it is the language that I am currently most comfortable with. I'm not going to go too far into detail, but I don't see myself dropping this language any time soon.

```python
from typing import List


def linearRegression(beta: List[float], x: List[float], y: List[float]) -> List[float]:
    # finding the means of the x and y arrays
    muX = sum(x) / len(x)
    muY = sum(y) / len(y)

    # finding the sum of squares using list comprehensions
    ssX = sum([(i-muX) ** 2 for i in x])
    sXY = sum([(x[i]-muX) * (y[i]-muY) for i, _ in enumerate(y)])

    # calculating the beta values
    beta[1] = sXY / ssX
    beta[0] = muY - (beta[1] * muX)


if __name__ == '__main__':
    # initializing the data
    x = [1, 2, 3, 4, 5, 6, 7]
    y = [1, 2, 3, 4, 5, 6, 7]

    # initializing the return object
    beta = [None] * 2

    # running the linear regression function
    linearRegression(beta, x, y)

    # printing out the results
    print(beta)
```

## C/C++
C has always been an interesting language to me purely due to the mystique around it when I first started coding. There is the manual memory management, the from-scratch data structures, and high level of abstraction that come together in this pliable language that can be used to create databases, servers, algorithms, applications, embeded systems ... this list can go on and on into any direction which makes it so powerful.

This obviously comes at the cost of being forced to create most of this stuff from scratch where complexity and safety can be real concerns. Even one of C's largest strengths, efficient memory and compute, can be a concern if things aren't done properly.

Following off of C, C++ is just C plus a little extra in the form of added data structures and modules that make abstractions easier. This still comes at a cost where learning the APIs and fighting back the complexity of C++ are the real problems.

With C/C++ being the oldest languages on the list there is alot of legacy code and different industries that rely heavily on them. The one that I am primarily interested in would the financial sector and algorithmic trading which relies on these languages for their low latency deciscion making. We also see the creep of these languages into machine learning and data science through the form of Python libraries such as `pandas`, `numpy`, and `tensorflow`.

```cpp
#include <iostream>
#include <vector>
using namespace std;

void linearRegression(float *beta, vector<float> x, vector<float> y)
{
    // initializing the values
    int n = x.size(), i;
    float muX = 0, muY = 0, ssX = 0, sXY = 0;

    // finding the mean of the x and y arrays
    for (i = 0; i < n; i++)
    {
        muX += x[i];
        muY += y[i];
    }
    muX = muX / n;
    muY = muY / n;

    // finding the sum of squares
    for (i = 0; i < n; i++)
    {
        ssX += (x[i] - muX) * (x[i] - muX);
        sXY += (x[i] - muX) * (y[i] - muY);
    }

    // calcuating the beta values
    beta[1] = sXY / ssX;
    beta[0] = muY - (beta[1] * muX);
}

int main()
{
    // initializing the data
    vector<float> x = {1, 2, 3, 4, 5, 6, 7};
    vector<float> y = {1, 2, 3, 4, 5, 6, 7};

    // initializing the return object
    float beta[2];

    // running the linear regression function
    linearRegression(beta, x, y);

    // printing out the results
    cout << "b0: " << beta[0] << " b1: " << beta[1] << endl;

    return 0;
}
```

## Go
Go is the language that I am most strongly drawn to in terms of it's mission. The website defines Go as an "open source programming language that makes it easy to build simple, reliable, and efficient software" where the idea of simplicity is what draws me in the most. 

Go has an extremely comprehensive standard library which allows software to be developed free of dependency (similar to C/C++) while also sticking to a strict style guideline which makes it very readable. Verbosity over *"magic"* is also appreciated within Go which is one of the main ideas that I follow throughout my code.

On the surface Go seems like the perfect language for me since it enforces all of the things that I find important in code (low dependecy, verbose, and easily read), but it's usage in industry are not directly involved within the areas that I want to work in. Go is primarily used within distributed systems and concurent designs where I hope to build analytical software or software that is used for edge computing.

```go
package main

import "fmt"

func linearRegression(beta *[2]float32, x []float32, y []float32) {
	// initializing the values
	n := len(x)
	var muX, muY, ssX, sXY float32 = 0, 0, 0, 0

	// finding the mean of the x and y arrays
	for i := 0; i < n; i++ {
		muX += x[i]
		muY += y[i]
	}
	muX = muX / float32(n)
	muY = muY / float32(n)

	// finding the sum of squares
	for i := 0; i < n; i++ {
		ssX += (x[i] - muX) * (x[i] - muX)
		sXY += (x[i] - muX) * (y[i] - muY)
	}

	// calcuating the beta values
	(*beta)[1] = sXY / ssX
	(*beta)[0] = muY - (beta[1] * muX)

}

func main() {
	// initializing the data
	var x = []float32{1, 2, 3, 4, 5, 6, 7}
	var y = []float32{1, 2, 3, 4, 5, 6, 7}

	// initializing the return object 
	var beta [2]float32
	
	// running the linear regression function
	linearRegression(&beta, x, y)
	
	// printing out the results
	fmt.Println(beta)
}
```

## Rust
Rust is the most interesting language to me on this list but it is also arguably the most complex and lesser implemented language. I think the potential of Rust is its most siginifanct factor due to all of the potential use cases given it's efficiency, safety, and recent support from larger companies. While I don't think that Rust will ever replace C/C++, there is the potential to use Rust in many of the same circumstances that you would consider C/C++ due to the fact that it has access to many of the same APIs, it is as efficient, and has the added factor of memory safety.

The potential of Rust being used for edge compute, which is something that I would like to work on, is massive given it's support by Mozilla and its ability to compile to WASM.

Learning Rust would be hedging my bets for the future on the potential but is really not a given like C/C++ and Go are.

```rust
fn linear_regression(beta: &mut [f32; 2], x: Vec<f32>, y: Vec<f32>) {
    // initializing the values
    let n = x.len();
    let (mut mux, mut muy, mut ssx, mut sxy) = (0.0, 0.0, 0.0, 0.0);

    // finding the mean of the x and y arrays
    for i in 1..n {
        mux += x[i];
        muy += y[i];
    }
    mux = mux / n as f32;
    muy = muy / n as f32;

    // funding the sum of squares
    for i in 1..n {
        ssx += (x[i] - mux) * (x[i] - mux);
        sxy += (x[i] - mux) * (y[i] - muy);
    }

    // calculating the beta values
    beta[1] = sxy / ssx;
    beta[0] = muy - (beta[1] * mux);
}

fn main() {
    // initializing the data
    let x = vec![1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0];
    let y = vec![1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0];

    // initializing the return object
    let mut beta: [f32; 2] = [0.0; 2];

    // runnings the linear regression function
    linear_regression(&mut beta, x, y);

    // printing out the results
    print!("b0: {} b1: {} \n", beta[0], beta[1]);
}
```