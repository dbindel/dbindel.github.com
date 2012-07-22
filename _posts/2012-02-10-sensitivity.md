---
layout:     post
title:        Automated sensitivity analysis
tags:       [technology, musings, math]
---

If I find an MEng student or undergrad with the right inclination --
or in the unlikely event that I get a windfall of time of my own that
isn't eaten by other projects -- I'd like to build a twist on the
simple calculator language.  I want a calculator that estimates
rounding errors throughout a calculation based on the standard
$(1+\delta)$ model, and uses sensitivity analysis to show how those
rounding errors propogate through the calculation.  This wouldn't be
all that much more complicated than the usual symbolic differentiation
stuff.  The only thing that's different is that one would effectively
be computing partial derivatives with respect to variables
representing the rounding error committed at each step of the
computation.

The cool thing about this is not that it would provide approximate a
posteriori error bounds.  I already know how to do that, and this tool
wouldn't really help with the hard part, which is doing an a priori
analysis to reveal where problems are likely to occur, and then being
clever in a way that guarantees adequate accuracy near those problem
cases.  No, the cool thing is that this could help my students
understand what I mean when I talk about a single critical rounding
error being amplified -- without me walking through the error analysis
at the board with them.
