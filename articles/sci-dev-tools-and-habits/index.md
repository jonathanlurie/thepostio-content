---
title: "Scientific Developer: My Tools and Habits"
date: 2020-10-06
excerpt: Here is how I work, the tools I like to use and how I approach a situation as a scientific developer.
cover: bird.jpg
tags:
  - scientific developer
  - tools
  - habits
---

*You can read the previous article of this series __"What's a Scientific Developer?"__ [here](/jonathanlurie/scientific-developer).*


# What kind of situation are we talking about?
In the [previous article](/jonathanlurie/scientific-developer), I have described roughly what it means to be working as a scientific developer, it was more about the *human aspect* and the soft skills, without getting too technical. Now is the time to get a bit deeper (but still, don't expect any code in this article!).  

The example I am going to use is one of the projects I am currently working on: a brain atlas.  
One may think *"oh, just a few brain mesh displayed WebGL and you're good to go!"*, and yes, that a part of it, but it's only the tip of the iceberg.  

# First, a bit of context
In the lab I'm working, there are many scientists and researchers. Counting all the staff, plus phd students, the postdocs, the interns, the visiting researchers... it's about 70 maybe. In addition, there are also many developers, with different expertise to sustain the different need the scientists may have (super-computing, machine learning, visualization, database, IT, etc.). Most of the time, we are grouped in small teams of 6 to 12 by topics of expertise, and those teams are generally full-dev or full-scientists with only rare exceptions. This means that scientists and developers are working together because there are collaborations between teams.  

Now let's say *Scientist A* needs a new library to crunch numbers in order to solve their problem, then they reach to *Team B* because it has this kind of expertise.

The brain atlas is an app that is expected by many of these teams, because it's a transverse topic: displaying spatially referenced data? Everything is somewhat related to space! And everybody wants to have their data displayed and shared among colleagues! More teams in the project involves means more requirements, more genericity, more architectural concerns.  

Here are some of the things we would like to have:
- choose what reference and species to display (mouse, rat, human?)
- have a fast faceted search to list data from the whole lab
- being able to display different types of datasets in the 3D playground
- download and access metadata of a particular dataset
- visualize datasets no matter how large they are
- be user-friendly
- use their credential to login
- visualize their data alongside data from other teams
- get specific information based on a 3D point (what brain region is that? What dataset is around?)

Then, under the hood, those can be translated like that:
- dig into the datasets and formats we are not familiar with
- store all the data in our data [graph database](https://bluebrainnexus.io/)
- make sure every dataset comes with loads of metadata to search on
- enforce metadata presence and shape with validation schema for a better indexing and fast query with ElasticSearch
- use web-friendly file formats
- write format parsers when they don't exist in JS
- create an Atlas-opinionated REST API
- create web-friendly clones from datasets that are not originally easy to decode from a browser
- downsample, chunk or tile the datasets that are too large for the web (or unnecessarily high resolution for visualization)
- Polish our web components and widgets. Use efficient frontend frameworks
- connect the dots on our graph database to make sure datasets from different teams can be seen together, in a standardized coordinate system
- prepare an architecture that scales because new formats and new modalities will hop in later (and we don't know how many)

This involves a fair amount of communication with the scientists and other developers to find the answers to all these questions. Also, some answers are mandatory since the beginning but some can wait. The tricky part is to know that it must be scalable but not really knowing in advance to which point.

# The fundamentals
linear algebra, gaussian, Fourier transform, exp/log, triangle, spatial geometry/plane/vector/intersection/aabb, trigonometry, statistics, machine-learning?, sampling, Pythagore/AlQuashi/Thales

I previously said that a scientific developer is *not* a developer who is also a scientist, but maybe I should have said *"who is not a researcher"*, because indeed, we have to know some bits of science. I call those *the fundamentals* because they are for me, and other scientific developers who work less with image processing, visualization and computer graphics may have a very different list of fundamentals. In other words, your mileage may vary, and that's ok to not tick all the following boxes.  

*__Warning:__ I am going to share Wikipedia links and I know Wikipedia math is a bit crazy and overwhelming, but it's just as a reference. Feel free to look up the very same terms on websites that actually care whether or not you understand what you are reading!*

## Basic math functions
This may sound trivial for developers that are well versed with the *functional paradigm* but I find very important to bear in mind what a *mathematical function* is: a machine that you feed with a number and turns it into another, reliably. The following functions are building blocks, to assemble and experiment with. The point is not to provide a list of the most complex functions but rather to demonstrate that what appear complex is, most of the time, only multiple simple things put together in a way that we have not figured out yet.

To create some behaviors that depend on a particular input, whether it's a distance, a volume, a number or occurrence per unit of time, you will be much more comfortable if you already know some basics and have an image of their curve in your mind. Among the very useful ones:

- the [affine](https://en.wikipedia.org/wiki/Affine_transformation) (and the linear), a straight line to simulate an increase or decrease in a very predictable fashion. *f(x) = ax + b*

- the [inverse](https://en.wikipedia.org/wiki/Inverse_function) to create behavior where your output gets closer and closer to zero but never quite cross the zero line. For instance, something that becomes transparent if you get too far. *f(x) = 1 / x*

- the [quadratic](https://en.wikipedia.org/wiki/Quadratic_function) to make a value grow tremendously (more than linear). This bell-curve function could be used to simulate the growing size of a blast, or, if you minus it, it could be the trajectory of an apple that you throw in the air. *f(x) = x²*

- the [polynomial](https://en.wikipedia.org/wiki/Polynomial) is a generalization of the quadratic, but instead of sticking to degree 2 (*x²*), it can be more. It's good to know this one exists but honestly, I don't use it every day.

- The [logarithm](https://en.wikipedia.org/wiki/Logarithm) assumed of *base 10* unless stated otherwise, the *base 2* is also quite handy. the *log of base a* is made to answer the question: "An arbitrary number is always *a* raised at *the power of something*, what is this *something*?". Now, I know that's a bit vague and also, who cares what power of what is this! So an example, maybe: How many digits in the number 9836012? Well, there is *floor(log10(9836012)) + 1* digits. Note that *log10(9836012)* is about *§.99* and as a comparison, *10^7* is *10000000*. Another example, using *base 2*, to convert a large number of bytes into a readable string [here](https://twitter.com/jonathanlurie/status/1298237099217432579). *f(x) = loga(x)*

- The [absolute value](https://en.wikipedia.org/wiki/Absolute_value), is basically when you just remove the sign and assume everything is positive. This is handy when you are in [frame of reference](https://en.wikipedia.org/wiki/Frame_of_reference) and want to compute the distance from one point to another. With absolute value, you don't need to care about what point is before or after, you arbitrarily decide of the point to go from and the point to go to and take the absolute distance. *f(x) = |x|*

That's all for the basics, and then you can combine all of them to get one you need! Whenever I am facing a situation where I know I will have to find a mathematical function to get the right behavior, the first thing I have in mind is usually the shape of the curve I need to create. Then, from that, and using the building blocks above, I approximate. This process usually takes a plotter such as *Grapher* (preinstall on all Mac computers) and there are also many online alternatives. Just type "function plotter" on Google, or better: build your own!

I have noticed that, over the years, a type of function I was using a lot are with [asymptotic behavior](https://en.wikipedia.org/wiki/Asymptotic_analysis?), which is when the input grows, the output grows as well but less and less and eventually become considered constant. Two functions I like, again, these are building block so twist them as you feel, are:

- the [sigmoid](https://en.wikipedia.org/wiki/Sigmoid_function), with this symmetric S-shape and asymptotic behavior on both end (which is not that common). *f(x) = 1 / (1 + exp(-x))*

- the i-am-not-aware-it-has-a-name one, but this one is mostly a tweaked inverse function, very handy to grow something rather naturally between 0 and 1, not to slow not too fast. To be used on R+ (aka. positive numbers). *f(x) = 1 / (x + 1)*

This list could grow exponentially based on the number of developers contributing to it (ok, maybe not exponentially), but that's already a good start. 


## Trigonometry
Being familiar with the [unit circle](https://en.wikipedia.org/wiki/Unit_circle) is a definite plus. The term *sine* and *cosine* may bring bad memories but without those two, hardly anything can rotate! If you want to tilt that mesh around this axis, then *cos* and *sin* are your friends. Knowing their reverse counterpart is also important because some computations give you the *sine* and *cosine* value and you must find the angle.  

An important part to remember is all the *radian to degree* conversion. Basically, 1 rad is 180 degrees (half a circle) and it's also Pi.  

If you need to create/draw patterns that have some redundancy or wavy components, then *sine* and *cosine* are probably going to be your friends as they are exactly that: waves, or oscillation, between 0 and 1.

If you take the usage of *cos* and *sin* the other way around, you can use it to check whether a list of 2D points that was given to you are in clockwise or counter-clockwise order. This situation is fairly common in computer graphics, where triangles must be described in a strict order to be visible. Wrong direction, and the triangle won't show up!



## Geometry, in 2D and 3D


## Linear algebra
Some may think "ouch, starting hard here!", but linear algebra is a big family and we only need a small part of it. For example

dot product, cross product

# Machine-related
(encoding of numbers, string, endianness, color representation, gpu/tesselation, ram/swap, http, chunking/tiling/lod)

# File formats
raw buffers, json, yaml, h5, pickle?, nrrd, tiff, csv

# Languages and libraries
c py js, notebooks?



[— @jonathanlurie](https://twitter.com/jonathanlurie)

Cover photo by [Thomas Millot](https://unsplash.com/@tmillot)