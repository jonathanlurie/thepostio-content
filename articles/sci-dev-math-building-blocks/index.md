---
title: "Scientific Developer: what math do you need?"
date: 2020-10-06
excerpt: A list of fundamentals and mathematics building blocks. And it's gonna be fine.
cover: bird.jpg
tags:
  - scientific developer
  - math
  - building-blocks
---

*You can read the previous article of this series __"What's a Scientific Developer?"__ [here](/jonathanlurie/scientific-developer).*


# What kind of situation are we talking about?
In the [previous article](/jonathanlurie/scientific-developer), I have described roughly what it means to be working as a scientific developer, it was more about the *human aspect* and the soft skills, without getting too technical. Now is the time to get a bit deeper (but still, don't expect any code in this article!). Then, this article can be used as a cheatsheet (mainly for all the links) or just as introductory material for a developer who would like to get into scientific software.

The example I am going to use is one of the projects I am currently working on: a brain atlas.  
One may think *"oh, just a few brain meshes displayed with WebGL and you're good to go!"*, and yes, that's a part of it, but it's only the tip of the iceberg.  

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

This involves a fair amount of communication with scientists and other developers to find the answers to all these questions. Also, some answers are mandatory since the beginning but some can wait. The tricky part is to know that it must be scalable but not really knowing in advance to which point.

# The fundamentals math things to know
I previously said that a scientific developer is *not* a developer who is also a scientist, but maybe I should have said *"who is not a researcher"*, because indeed, we have to know some bits of science. I call those *the fundamentals* because they are for me, and other scientific developers who work less with image processing, visualization and computer graphics may have a very different list of fundamentals. In other words, your mileage may vary, and that's ok to not tick all the following boxes.  

*__Warning:__ I am going to share Wikipedia links and I know Wikipedia math is a bit crazy and overwhelming, but it's just as a reference. Feel free to look up the very same terms on websites that actually care whether or not you understand what you are reading!*

## Types of numbers
Yes, here we start by the beginning! We've all learned about that, but for most of us, it was a very long time ago. So, to make it short, and because we usually don't need all the categories, there are:
- the Natural numbers: whole and positive (0, 1, 2, 3, etc.)
- the Integers: whole, positive or negative
- the Real numbers: more or less the numbers with a decimal part (that can be 0). I know this is not exactly correct, but for the job, we do not truly make a difference between *rational* or *irrational*

This notion is not to be skipped and we'll see later the relation with encoding and why it matters.

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

- the [sigmoid](https://en.wikipedia.org/wiki/Sigmoid_function), with this symmetric S-shape and asymptotic behavior on both ends (which is not that common). I recently used this one in the brain atlas to make the meshes progressively transparent as the camera is getting closer. *f(x) = 1 / (1 + exp(-x))*

- the *i-am-not-aware-it-has-a-name one*, but this one is mostly a tweaked inverse function, very handy to grow something rather naturally between 0 and 1, not to slow not too fast. To be used on R+ (aka. positive numbers). *f(x) = 1 / (x + 1)*

This list could grow exponentially based on the number of developers contributing to it (ok, maybe not exponentially), but that's already a good start. 


## Trigonometry
Here are some functions that I haven'tmentioned above because I though they deserve their own category!  

Being familiar with the [unit circle](https://en.wikipedia.org/wiki/Unit_circle) is a definite plus. The term *sine* and *cosine* may bring bad memories but without those two, hardly anything can rotate! If you want to tilt that mesh around this axis, then *cos* and *sin* are your friends. Knowing their reverse counterpart is also important because some computations give you the *sine* and *cosine* values so that you can find the angle (eg. *dot product*).  

If you need to create/draw patterns that have some redundancy or wavy components, then *sine* and *cosine* are probably going to be your friends as they are exactly that: waves, or oscillation, between -1 and 1.

The other way around, *cos* and *sin* can be used to check whether a list of 2D points are given in clockwise or counter-clockwise order. This situation is fairly common in computer graphics, where triangles must be described in a strict order to be visible. Wrong direction, and the triangle won't show up! (this is related to the normal of the plane defined by the triangle, but we'll talk about that later)

There are many relations between trigonometry and other topics, it's everywhere and the more I think of it the more I realize everything is somehow related to sine or cosine: [geometry](https://en.wikipedia.org/wiki/List_of_trigonometric_identities), [signal processing](https://en.wikipedia.org/wiki/Fourier_transform), [exponential function](https://en.wikipedia.org/wiki/Euler%27s_formula). I can only advise to scroll though the [Wikipedia page for it](https://en.wikipedia.org/wiki/Trigonometry), even though I am aware there is almost as many links as there words in this article!


## Geometry, in 2D and 3D
Geometry can be quite complex and the more dimensions you add, the more difficult it is to produce a mental representation of a problem. But, the good thing with geometry is some notions are still rather accessible. Let's start with those!  

Some notions are always the same, no matter how many dimensions we are dealing with:  
- you need a [coordinate system](https://en.wikipedia.org/wiki/Coordinate_system), most of the time, it's going to be a *Cartesian* one. Basically, an *origin* and as many axes as there are dimensions. Note: axis are always orthogonal to each other in a Cartesian coordinate system (this is why you can't have more axes than dimensions).  

- A *point*. It's the most minimal feature you can get. It's just there floating in your coordinate system

- A *line* is infinite and straight. Because it's infinite, it does not have a point to mark its beginning or its end, though, an infinite amount of points are laying on it. If you know two points on this line, you can [find the equation](https://en.wikipedia.org/wiki/Linear_equation). This equation, *y = ax + b* (Oh! That's the *affine function* definition!) is handy if you need to find the intersection between a ray (or line) and another geometric entity, such as a plane, a sphere, etc.

- a *line segment*, is defined along a line but has a beginning and an end, marked by two points. In other words, it's a line between two points.

- a *triangle*. I suppose everybody knows what a triangle is but just in case: it is a shape composed of 3 points, linked together by line segments. The triangle is the [*simplex*](https://en.wikipedia.org/wiki/Simplex) of the 2D space because its points are all *affinely independent*. In other words, a triangle in a 2D space (or more generally, a simplex in its N-dimensional coordinate system) is exploiting all the dimensions available, no matter how rotated it is. Just like the sine and cosine functions we mentioned above, triangles are everywhere and thousands of theories are based on it. Just scroll the [Wikipedia page](https://en.wikipedia.org/wiki/Triangle) to get a taste of it!

Because triangles are the simplest shape to have a non-null area, they can be used to compose any other geometrical shapes, at least as an estimation. For example, 3D meshes to create all sorts of realistic shapes, are very often composed of thousands of very small triangles.

You have probably learned about some triangle-related theorems in class when you were a teenager, I am thinking mostly of the [intercept theorem of Thales](https://en.wikipedia.org/wiki/Intercept_theorem), [Pythagore](https://en.wikipedia.org/wiki/Pythagorean_theorem) and the lesser-known [Al-Kashi](https://en.wikipedia.org/wiki/Law_of_cosines) (which is a generalization of the Pythagorean theorem that works for non-right triangles). I must admit that those three, are probably the geometric tools I have used the most in my life! They are quite simple to apply, they work in N-dimensional spaces and are incredibly cheap in terms of computation.

- a *bounding box*, and more precisely an [*axis aligned bounding box*](https://en.wikipedia.org/wiki/Minimum_bounding_box#Axis-aligned_minimum_bounding_box) (also called *AABB*). We can see it as a frame (in 2D) or really as a box (in 3D) that delimits a certain area and is generally used as a container of things that we want as small as possible. For example, let's say you have a set of triangles that are grouped because they may represent something together (a player's character in a game) and at some point, you want to know if a line (a laser, a bullet, etc. ) is intersecting it. Then if you intersect the ray with the box, you can very quickly spare yourself with the intersections with all the triangles.

- a *vector*. This seems a bit more abstract than the above but to put it simply, we can see a [vector](https://en.wikipedia.org/wiki/Euclidean_vector) an arrow (the Robin Wood kind of arrow), it has a length, an orientation and a direction (as a bidirectional arrow sounds like a risky device to use!). Just like an arrow, if you move or rotate a vector, it length is not changing. One of the purposes of vectors is to symbolize a direction and in some cases, we don't really need to know how long they are, we just need to know where they are pointing to. Because of that, and also to not alter some computations with a factor other than *1*, we often use [*normal vectors*](https://en.wikipedia.org/wiki/Normal_(geometry)), they are just like regular vectors except they have a length of *1*.

Now that we are ok with the basics of geometry in 2D/3D, we can explore a bit more a very 3D-specific feature: 

- *planes*. In a way, the plane, in 3D, is the equivalent of the line in 2D: it's infinite, does not have a beginning or an end, yet an infinity of points are laying on it, it has an orientation. And just like 2 points are enough to define a 2D (or 3D) line, 3 points are enough to define a plane. In other words, if you have a triangle, you have a plane. But there are other ways to define a plane. The one I like the most is with a point and a [normal vector](https://en.wikipedia.org/wiki/Normal_(geometry)). I find it fairly easy to get a representation of this method. Take a sheet of paper (or cardboard), plant a pencil in the middle, and change the orientation of the whole thing without translating the center point. (yeah, it's actually easier to just do it mentally!). Planes, just like lines, can be represented by a function: *ax + by + cz + d = 0* . Note that there are many ways to [find the plane equation from geometric features and vice-versa](https://en.wikipedia.org/wiki/Plane_(geometry)). It's not always worth remembering them all by heart but it's very handy to know they exist so that you instantly think "I know this, I can find that".

So why such a long part on planes and why do we care so much about this? Again, this is very dependant on they topic you will work on, but more often than not, when you need to do spatial geometry, you need to compute intersections between different kinds of features and for a matter of performance, it is usually faster to compute the intersection with a plane first.   

Another important use of planes is to separate other features, just like we cut a loaf of bread. For example, the space partitioning method called [*bounding volume hierarchy*](https://en.wikipedia.org/wiki/Plane_(geometry)) is using planes to separates the triangles of a mesh more or less equally between its two sides. Then, two bounding boxes are created to encapsulate each part, and are again split by other planes and so on. Just like [*quadtrees*](https://en.wikipedia.org/wiki/Quadtree) and [*octrees*](https://en.wikipedia.org/wiki/Octree), the purpose of a *BVH* is to find ab object in space. But this topic deserves its own article!


## Linear algebra: matrices and vectors
Again, this is a very large family, and we are definitely not going to cover it all, only the most common bits that I am playing with on (almost) a daily basis: vectors and matrices of size *2x2*, *3x3* and *4x4*, in the context of a vector space. This decision is mainly motivated by the fact that such vectors and matrices are used to characterize [affine transformations](https://en.wikipedia.org/wiki/Affine_transformation) that a moving points around.

The brain atlas I am working on requires a 3D environment, and the most common type of matrices for [homogeneous transformations](https://en.wikipedia.org/wiki/Transformation_matrix) in this case are *4x4* matrices. And because of that, vectors (to symbolize a point in space or a translation) are not composed of 3 elements (x, y, and z) but of 4, the last element is generally 1.

A *4x4* matrix can represent different kinds of transformations:
- no transformation, this is an identify matrix
- **translation**, when you move a point of a given distance in a given direction
- **scaling**, to make elements smaller of bigger by a given factor.
- **rotation**, to twist things
- **shearing**, to squeeze things along an axis more than another

Note that scaling and rotation are effective from the origin. You always rotate things around a point (in 2D) or around an axis (in 3D). For scaling, even though it is less obvious because we have fewer real-life examples of things that we shrink or grow, but imagine you are holding an old inner-tube from your bicycle in your hands, and you pull with your right hand, trying to keep your left hand static. Then the left hand is the origin.

In terms of optimization, it's good to know that you can multiply matrices together and end up with a single matrix that will represent them all. Then you can just use this one to apply to all the points you need to modify.  

- [*Matrix-vector multiplications*](https://en.wikipedia.org/wiki/Matrix_multiplication). In an Euclidian 3D vector space, you sometimes want to perform this kind of operation to apply a spatial (affine) transform (the **matrix**) to a point in space (the **vector**). Of course, if you need (or want) to multiply an arbitrary matrix with an arbitrary vector and this has nothing to do with anything spatial, no problem, but we have to respect a rule: the vector must have has many rows as the matrix has columns. This kind of operation is applied to individual points.

- [*Matrix-matrix multiplications*](https://en.wikipedia.org/wiki/Matrix_multiplication). The most common situation is to group transformations in order to end up with a unique matrix that represents the whole transformation. Though, you must be careful with that because the order matters! I find this course material ([part1](http://web.cse.ohio-state.edu/~wang.3602/courses/cse5542-2013-spring/5-TransformationI.pdf) & [part2](http://web.cse.ohio-state.edu/~wang.3602/courses/cse5542-2013-spring/6-Transformation_II.pdf)) from Uni of Ohio very well done on this tricky topic.

- The *world matrix* may be different for each object in the scene and is the one that transforms a point from its own space into **world space**. This notion is not trivial so let's use an example: you are comfortably sitting on your couch and your bicycle is getting dust in your garage. if we take your own self as a reference for yourself and we take your bicycle as the reference for your bicycle. We can safely say that you are both at the coordinate **(0, 0, 0)** of your respective coordinate systems, and yet, you are not physically on your bicycle. Now if we establish (arbitrarily) that your mailbox is going to be the origin for everything, then your position and the position of your bicycle are different. You will have your own world matrix (your translation from the mailbox), and the bicycle with also have its own world matrix (its translation from the mailbox). Next step, you decide to take you bike for a ride, then you have more or less the same world matrix than your bike, since you are sitting on it, but what about the valve of the front wheel of your bicycle? Is it the same as your bicycle? No, because it's spinning around an axis that is not directly tied to your mailbox (the world reference, remember) but to your bicycle. In other words, the valve has a transformation matrix that is **not** a world matrix. It can be seen as a nested object and such matrix is called a *model matrix*. To find the position of the valve in world space, the valve model matrix must be multiplied by the bike world matrix.

- [*Dot product*](https://en.wikipedia.org/wiki/Dot_product). This is used for many different things and is highly related to both vectors and *cosine*. In my case, I use dot products exclusively between unit vectors. Why? Because it's a super economical way to check is two vectors are going towards the same direction (dot product equals *1*), towards opposite directions (dot product equals *-1*) of if they cross orthogonally (dot product equals *0*). This may remind you a lot about the kinds of value the unit circle was showing, and this exactly because the dot product between two unit vectors is also the *cosine* or their respective line segments if they were stuck end-to-end. Handy.

- [*Cross product*](https://en.wikipedia.org/wiki/Cross_product). This is sometimes confused with the one above, though those are quite different. Again, I exclusively use the cross product with a pair of vectors that I have normalized (transformed into unit vectors). Then, the result of a cross product is the vector that is orthogonal to the two. We said earlier that a plane in a 3D world could be determined with 3 points (A, B and C, you can draw them on the floor). Now imagine you draw a vector AB and a segment BC, then the cross product *AB x BC* is the vector that is perpendicular to the floor, basically, it's you (from shoes to head). If AB and BC are normalized, then the result is also normalized and is called the **normal vector**. In computer graphics, the normal vector (alongside dot product) is involved in the computation of most light models, one of the simplest being the [Lambertian reflectance](https://en.wikipedia.org/wiki/Lambertian_reflectance).  

There are plenty of matrix-oriented notions that are good to know, such as the existence of the [*identity matrix*](https://en.wikipedia.org/wiki/Identity_matrix) or that a matrix [determinant](https://en.wikipedia.org/wiki/Determinant) equaling zero means the transformation is invalid and is squishing the whole thing into a lower dimension. There is a [very good video](https://www.youtube.com/watch?v=Ip3X9LOh2dk&vl=en) by [3Blue1Brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw) about this last bit.

## Statistics
We all do at least a bit of statistics in our everyday life, sometimes without knowing it. "Average" or "mean" is generally the first that comes to mind but when we say things such as *"Did I put too much sugar in your coffee?"* or *"It really is too far to walk there, I'd rather take the bus."*, then we are also doing some quick math in our head. What do **too much** and **not enough** even mean? Compared to what? Those notions can quickly become crucial to give insights about a tolerance or a risk.  

### The classics
- A [*population*](https://en.wikipedia.org/wiki/Statistical_population) is a group of things/entities/people/elements/whatever that we want to study. A population is generally defined by some criteria such as their location, some biological trait, etc. but usually not directly by the variable you want to study. For example, if you want to compute statistics about the wingspan of the bald eagle in a specific region, then you should not add criteria to frame the population such as *"With wingspan above 5 feet"*, otherwise, your study will suffer from a [*samling bias*](https://en.wikipedia.org/wiki/Sampling_bias). It worth mentioning that a **whole population** is often impossible to study because we can only collect data about the whole population very rarely. So what? Well, we'll say we are studying a [*sample*](https://en.wikipedia.org/wiki/Sample_(statistics)) instead. In terms of maths, some formula may vary because of that so pay attention to the **N** being replaced by **(N - 1)** or this kind of trick.

- The population size **N** is the number of things/entities/people/elements/whatever in your study. That's all.

- A thing/entity/person/element/whatever is said [*univariate*](https://en.wikipedia.org/wiki/Univariate) when it has only a single variable to study, or that we want to study only a single variable in it. All the following will be about univariate things.

- The [*mean*](https://en.wikipedia.org/wiki/Mean) (greek letter *μ*, "mu"), and in particular the *arithmetic mean* may sound a bit trivial but it's not a reason to skip it! You can read about the other kinds of *means* as well. Bear in mind that the mean is very affected by [*outliers*](https://en.wikipedia.org/wiki/Outlier), so if you have a mistake in your sample, your mean will suffer from it.

- The [*median*](https://en.wikipedia.org/wiki/Median) is a more robust metric than the mean for a population that may contain outliers. As a result, half of the population will be below the median and half will be above. In case of doubt, prefer the median to the mean.

- The [*percentiles*](https://en.wikipedia.org/wiki/Percentile) are just like the median but instead of looking the value where 50% of the population is below and 50% is above, you will be looking for the value at which 10% is below and 90% is above (the first percentile), then 20% is below and 80% is above, etc. Following the very same logic, *Q1* and *Q3* are the first and third [*percentile*](https://en.wikipedia.org/wiki/Quartile), respectively for 25% and 50% (and the median is then called *Q2*). Sometimes, we like to compare or display the first percentile (10%) with the minimum value and the last percentile (90%) with the maximum value to emphasize on how elongated are the tails of the dataset.

- The [*standard deviation*](https://en.wikipedia.org/wiki/Standard_deviation) (STD or greek letter *σ*, sigma) is one of the most important metrics when it comes to defining a threshold of a range of tolerance (so a upper and a lower threshold). It is always to be used with the *mean*. For example, if your variable follows a *normal distribution* (see below), then a rather safe range of tolerance for what can be considered a "a not-too-crazy-OK value" starts at *mean - A * STD* and ends at *mean + A * STD* and depending on how conservative you are, *A* can be *1*, *2* or *3* (but not more, we'll see later why).

Saying too much about the *standard deviation** without mentioning the concept of [*normal distribution*](https://en.wikipedia.org/wiki/Normal_distribution) is difficult, and yet mentioning "normal distribution" but not "standard deviation" would be kind of stupid, so here we are! From a statistical point of view, being able to plot the Gaussian bell curve is definitely not needed so you don't need to remember the formula. To put it simply, the normal distribution is generally a reliable model to find the probability of a given event that follows somewhat of a natural law, knowing some data in advance. Pretty vague, right? And yet, it's quite powerful and it's used everywhere!

There are two main ways of using a normal distribution:
1. You know very well your population, you have actual measures and want to extract some insight and meaning
2. You have no actual measure but a rather good understanding of what you are studying and knowing that your variable follows a normal distribution, you can simulate a synthetic sample

The two parameters of the bell curve are the *mean* (*μ*) and the *standard deviation* (*σ*), you can see it from the fo



https://www.wikiwand.com/en/Fat-tailed_distribution
https://www.wikiwand.com/en/Long_tail
https://en.wikipedia.org/wiki/Median_absolute_deviation

# Sampling
Nyquist–Shannon
nearest neighbors
linear
cubic
spline


# Decompositions and transforms
frequencies, FFT, wavelet


[— @jonathanlurie](https://twitter.com/jonathanlurie)

Cover photo by [Thomas Millot](https://unsplash.com/@tmillot)