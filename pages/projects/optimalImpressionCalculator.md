---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /projects/optimalImpressionCalculator/
categories: projects
---

## Optimal Impression Calculator for Multi-Stage Advertisement Campaigns on Social Media

This calculator utilizes a dynamic approach to optimally (maximization) allocate advertisements across a Bi-stage Advertisement Campaign and calculate the expected number of clicks for that assignment.
![alt text](/img/oicsc.png "Screenshot of Program Running")
_Optimal Impression Calculator_

### Operation

Basically the scenario is this: For a given person (node) in an Online Social Network (OSN) who has friends represented by edges connected to another node; the probability of that node clicking an advertisement is partially dependent on the actions of that node's friends when they are presented with the same advertisement. So think of a someone named Tom who has three friends: Kim, Joe & Jill. Suppose the advert was shown to Joe intially and Joe clicked on it. Because of this, the likelihood of Tom clicking the advert will be increased.

The equation to update the probability of a given node is defined as follows:

![alt text](http://latex.codecogs.com/gif.latex?p%20%5Cleftarrow%20max%5C%7B0%2C%20min%5C%7B0.25%20+%20%5Calpha*%28%5Cfrac%7Bn%7D%7Bf%7D%29%20%5C%7D%20%5C%7D "Update Probability Equation")

Where alpha is a given scaling constant, (in this case 0.15). n is the total number of friends of a given node that clicked an impression when it was shown to them. f is the total number of a friends for the user under examination.

For more information about this check out this paper: [On the Problem of Multi-Staged Impression Allocation in Online Social Networks](https://link.springer.com/chapter/10.1007/978-3-319-89932-9_4), done by [Inzamam Rahaman](https://www.linkedin.com/in/inzamam-rahaman-721b40b8/) and [Patrick Hosein](http://hosein.tt).

The front end was created entirely utilizing Tkinter, with matplotlib being used for graphing the networks. The backend of the program was done using an object oriented approach and [NetworkX](https://networkx.github.io/). To determine the optimal allocaiton; all allocations were have to be considered. The basic flow diagram for calculating the Expectation of the number of clicks based on the impression allocation is shown below.

![alt text](/img/oicfl.png "Flow Diagram of Calculating Expectance")
_Flow Diagram for an assignment of [2,3] at the first and second stage_
