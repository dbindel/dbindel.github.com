---
layout: post
title:  Topics in Julia
tags:   [teaching, technology]
---

I learned about [Julia](http://julialang.org) when it was first
announced in early 2012.  Despite knowing and respecting some of the
names behind it, and despite several friends ribbing me about a new
numerical language that shared a name with my daughter, I took a
wait-and-see attitude at first.  But I kept an eye on Julia, and I saw
as many positive signs as I saw hyped stories.  A community formed,
and people other than the language designers started writing packages.
It picked up the ability to call Python, and with that the ability to
access the marvelous Matplotlib library.  And the numerical folks at
MIT had started using it for classes.  So by Fall 2013, I was ready to
give Julia a try myself.

This spring, I taught [CS 5220: Applications of Parallel
Computers](http://www.cs.cornell.edu/~bindel/class/cs5220-s14/), a
master-level course on high-performance scientific computing.  As in
past offerings of the class, I gave a few conventional assignments in
which students were to tune a base serial code that I gave them, then
parallelize the code using MPI or OpenMP.  And as in past offerings,
most of the assignments involved standard numerical kernels or
simulation methods.  But every time I offer this course, I try to give
at least one assignment with a less classically numerical feel, and at
least one assignment with a different style of parallelism.  This
semester, I satisfied both these desiderata by having the students
write a [topic mining algorithm in Julia][5220-hw3].  I figured this
would be a good way to test the claim that it is straightforward to
write code with good performance in Julia.  In the end, I think I
figured correctly.

As with the other assignments, I started off by providing the students
with a [base code][hw3-code] and a data set of Yelp reviews that was
kindly provided to me by David Mimno and Moontae Lee.  The students had
two main tasks:

1.  Replace a nonnegative least squares routine (an active set algorithm)
    that I provided with a faster, if less accurate, exponentiated gradient
    solver.
2.  Parallelize the almost-embarrassingly-parallel part of the code

My first surprise came when I started implementing the base code.
I'd done one previous programming exercise in Julia,
when I wrote the HTCondor cluster manager over the winter break;
I figured we would need this for the class anyhow, and that was
a good enough excuse.  But this was the first time I had done
any numerical programming in Julia, and I expected to spend a bit
of time stumbling around in the dark before I figured out what
I was doing.  As it happened, I wrote the code almost as I would
write Matlab code, and with surprisingly little debugging effort.
The greatest difficulty I had was not with the language, but
with choosing the parameters in the exponentiated gradient algorithm
in order to get acceptable performance.  I was sufficiently concerned
about the correctness of my own implementation that I decided to write
the active set routine, even though that was not part of the original
plan.  And at the end of writing both routines, cleaning up the parts
that I thought might be confusing, and adding some commentary,
I already felt fairly comfortable with Julia.

My second surprise came before I posted the assignment: my Julia
code processed the Yelp data set in a couple seconds, fast enough that
I was concerned that my students would not be able to get much
parallel speedup.  So I added some pointers to a few larger data sets,
as well as some code to process those data sets.

My third surprise came after I posted the assignment: with a little
fiddling, my active set solver was running faster than my own
exponentiated gradient solver (on which I'd admittedly spent less time
and effort), at least when we were trying to learn a small number of
topics.  This left me doubly concerned, since the assignment indicated
that it should be easy for the students to beat my performance with the
alternative solver.  Fortunately, while a few students did indeed need
reassurances, other students did a better job at tuning the exponentiated
gradient code than I had, and got the expected performance gains.

When the students really engaged with the assignment, they did about
what I thought they might do.  Most got the basic algorithm working
and managed at least some parallel speedup, and some managed to
significantly improve the performance of the base code that I
provided.  In the process, though, they ran into issues that I
probably would not have stumbled across in quite some time.
In no particular order, the ones that come to mind are:

*Documentation*.
  The biggest complaint among the students was the lack of clear
  documentation of some important language features.  In particular,
  the students were not that happy with the documentation on how Julia
  handles parallelism.  Several students also looked up the documentation
  for the wrong version of the language, which didn't help matters any.
  Also, there are few enough examples online that many students still
  felt in the dark after spending some quality time with Google.

*Version differences*.
  I installed the most recent pre-release of Julia 0.3 on the cluster.
  Several students used Julia 0.2 on their laptops.  The language is
  evolving fast enough that there were differences between these releases
  that caused some woe.  Perhaps the most memorable real difference had
  to do with elementwise subtraction of vectors, which could be done
  with `-` or `.-` operators.  Ordinary subtraction was deprecated in
  Julia 0.3, and at least one group ran into a tremendous slowdown on
  the cluster (compared to their laptop) because of the I/O overhead
  associated with Julia warning them millions of times that they should
  now use `.-` instead of `-`.

*Parallel profiling*.
  Most of the class settled on the same types of spawn-and-fetch
  strategies for parallelizing the main loop that I asked them to tackle.
  But none of us were able to get great insight into the parallel
  overheads by any mechanism other than careful manual instrumentation
  with timers.

*JIT overhead*.
  The first time it invokes a function, Julia uses a Just-In-Time
  compiler to compile the routine down to machine code.  The JIT
  compilation takes some time, and unless one wants to measure that
  time, it's important to run the code twice and time the second
  trial.  I told the students about this, but many of them ignored me.
  That in itself is no surprise; what is more surprising is how much
  variance the students discovered in the JIT time.  One group in
  particular was puzzled at an apparent 2x slowdown in their parallel
  code that turned out to be entirely due to the JIT processing their
  parallel code more slowly than their serial code.

*Vectors and array slices*.
  For the most part, Julia's performance conformed to my mental model.
  But one student group pointed out something that surprised me.  They
  saw significant speedups by copying a column of a matrix into a
  temporary vector, operating on that temporary vector in place, and
  then copying the results back.  I always just operated on the column
  of the matrix in place.  Given that Julia is column-oriented, so
  that the storage layout should be the same in both cases, I'm still
  a bit confused about why their approach improved performance so
  much.

*OpenMP and OpenBLAS*.
  Somewhat to our surprise, setting the `OMP_NUM_THREADS` environment
  variable significantly changed the performance of some students'
  serial codes.  The really surprising aspect of this is that it didn't
  seem to matter whether the variable was set to 1 or to 8 (the number
  of cores on the cluster nodes).  Just setting it to *something* made
  a difference.  We compiled Julia to use OpenBLAS, which is capable
  of running in multi-threaded mode for serial codes, though that
  should be turned off when Julia is running several local worker
  processes.  But I still don't understand why it would run more
  faster when the number of threads is explicitly set to the number of
  threads it ought to choose anyhow (either one or the number of
  cores).

I was happy that most of the students were able to use the profiler
and make some sense of its output.  Many did not figure out the how to
fix the bottlenecks that the profiler revealed, but as often as not
this was a matter of how they were thinking about the algorithm and
not how they were using the Julia language.  The student codes were,
as always, of varying quality; but they mostly submitted code that was
free of major performance gaffes.

Most of the students said nothing about their feelings on Julia in the
assignment reports, though I know from conversations in office hours
that several were frustrated from running into what they saw as
incomplete or undocumented corners of the language.  Perhaps I'll
read more about this when the course evaluations come in.

[hw3-assign]: http://www.cs.cornell.edu/~bindel/class/cs5220-s14/hw3.pdf
[hw3-code]: http://www.cs.cornell.edu/~bindel/class/cs5220-s14/html/anchor_topic.html

