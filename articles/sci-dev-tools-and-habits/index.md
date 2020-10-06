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

## Trigonometry
Being familiar with the [unit circle](https://en.wikipedia.org/wiki/Unit_circle) is a definite plus. The term *sine* and *cosine* may bring bad memories but without those two, hardly anything can rotate! If you want to tilt that mesh around this axis, then *cos* and *sin* are your friends. Knowing their reverse counterpart is a good asset too.  

An important part to remember is all the *radian to degree* conversion. Basically, 1 rad is 180 degrees (half a circle) and it's also Pi.

## Linear algebra
Some may think "ouch, starting hard here!", but linear algebra is a big family and we only need a small part of it. For example

# Machine-related
(encoding of numbers, string, endianness, color representation, gpu/tesselation, ram/swap, http, chunking/tiling/lod)

# File formats
raw buffers, json, yaml, h5, pickle?, nrrd, tiff, csv

# Languages and libraries
c py js, notebooks?



[— @jonathanlurie](https://twitter.com/jonathanlurie)

Cover photo by [Thomas Millot](https://unsplash.com/@tmillot)