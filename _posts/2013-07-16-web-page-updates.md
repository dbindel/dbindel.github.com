---
layout: post
title:  Web page updates
tags:   [technology]
---

I last touched [my web page](http://www.cs.cornell.edu/~bindel/) over
a year ago.  It has been longer since I did any work on the design.
The result was embarrassingly out-of-date.  Moreover, the
[webby](http://webby.rubyforge.org/) generator that I used to create
the site is [no longer maintained](https://github.com/TwP/webby).  I
wanted to clean things up some time before the end of the summer,
ideally before setting up my class web page for the fall.  I wanted a
basic, static design, ideally one that would not take much effort to
set up and would be easier to maintain than the old version.  This is
ideal work for when I'm sleep-deprived, distracted, or otherwise lack
focus -- which means right after my return from a trip is an ideal time.

I tried to follow a few guiding principles in the redesign:

- It should be visually appealing and portable across browsers.  That
  takes work, and visual design is not my strong suit.  So I wanted to
  start from an existing framework for the page design.
- The home page should be compact, with some news, highlights, and
  current classes.  Other pages should also be short.  Long lists and
  text blocks are easy to write, but hard to digest.
- I wanted conversion from my current system to be low-overhead.
  That meant using a system where I could continue to use 
  [Markdown](http://daringfireball.net/projects/markdown/) to format
  content, and something reasonably familiar for layout.
- I wanted maintenance to be a low-overhead task.  That means, in
  particular, following the
  [DRY principle](http://en.wikipedia.org/wiki/Don't_repeat_yourself).

After deciding on the principles, the implementation was
straightforward:

- I used the [Twitter Bootstrap](http://twitter.github.io/bootstrap/)
  framework for the basic HTML/CSS framework.  It's pretty, and it's
  designed by people who know far more than I about typography and
  about web portability.
- I set up "blurb" pages for projects and classes, and decorated each
  with metadata including a picture link, a short title, and a short
  teaser (no more than a line or two).  These metadata elements are
  used to construct the summaries that appear on the home page.
- I used [nanoc](http://nanoc.ws/) as the basic engine for compiling
  the redesigned pages.  The system is similar to what I used with
  webby, and so I was able to move content from the old version of
  the site without too much work.  I still need to update some of that
  older content, but that's an issue orthogonal to the general
  redesign -- and at this point, I figure that an incomplete update is
  better than none at all!
- I added some Ruby helpers to extract and rearrange material in order
  to automatically generate everything (front page and detail pages)
  from a single set of content files.  Part of this framework involves
  automatically pulling out which items ought to go on the front page;
  we'll see as time goes on whether I like the logic for this or want
  to do something else.

Since I slept reasonably well last night and have other things to do,
it's time to [put this up](http://www.cs.cornell.edu/~bindel/) and
move on.  I'm pleased it only took about half a day longer than I
anticipated, and I'm pleased with the look of the new design.  We'll
see what I think of it over time.
