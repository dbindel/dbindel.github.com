---
layout:     post
title:      Cheap, fast, or good?
tags: musings
---

There's an old saying about software projects (and perhaps about
other projects as well).  You can have it cheap, fast, or good --
pick any two.  I thought about this yesterday while I was putting
together the first lecture for my parallel computing class.  These
words mean different things in different contexts.  In commercial
software design, "cheap" might refer to money, "fast" to development speed,
and "good" to the robustness and features of the project.  In parallel
scientific software, "cheap" might refer to development effort (paid for in
time or money), "fast" might refer to time-to-solution, and "good" might refer
to the approximation error in the computed solution.  But a funny thing happens
in parallel software, which is that you have to go through every combination
in order to get fast-and-good.  Cheap-and-fast estimates are a staple of
engineering estimates, and it's just good practice to have a rough idea of
what your answers will look like before starting any computation.
Cheap-and-good (but slow) codes are extremely helpful in testing the accuracy
of more sophisticated methods, even if they can only be used on small problem
instances.  So to develop a fast-and-good parallel scientific code, you want 
to first do the cheap things, just so that you don't destroy the quality of
your computed answers by bugs introduced in the quest for better parallel
performance.

Of course, a remarkable number of attempts to build fast parallel codes
produce results that are neither cheap, nor fast, nor good.  And this is 
part of why I teach my class.

