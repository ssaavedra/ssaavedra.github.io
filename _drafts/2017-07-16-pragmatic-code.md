---
layout: post
title: "An important thought on the creation of frameworks, libraries and software"
language: en
categories:
  - Programming
  - Philosophy
tags: opinion-based
---

Thinking on the API from the inside or outside.

I was reading Aaron Swartz’s thoughts about their [Reddit rewrite in 2005](http://www.aaronsw.com/weblog/rewritingreddit), and I consciously realized something that I could only “feel” unconsciously before. Something that I think every software developer should at least be acquainted with.

I think it is much easier to start feeling it if you begin to program in Lisp. Maybe that’s because the language get’s much less in the way (there is pretty much no syntax). But, of course, your mileage may vary.

We are currently in the morning of a *post-TDD culture*. Usually, people tend to agree that writing tests and specifications before the actual implementation is a positive change, that –even though does not require less time– usually ends up with more quality software, quality meaning less bugs.

However, we also live in an *API world*, where most information is to be consumed from a plethora of mechanisms, IoT and whatnot.

So external APIs are usually given a bit of a though in the programmers’ heads, and there are even job postings about “Senior API Programmer” and the like, which emphasizes my point that programming interfaces are important.

However, many *internal* APIs are not that much well-thought and then it is a pain to program in that software.

How does that happen?

I think it is related to the process of how your language and your environment forces you to think. So, people thinking on the short term may first think on how can your feature be accommodated to your environment in the easiest way, and after that has been engineered and tested, an API is exposed, cutting its way through the environment-dependent thought you had.

However, there is a much better way of developing software, and this may sound totally useless, but it is nice to write it down. In Swartz’s words: I imagined how things should work and then I made that happen.

That is,

> First imagine how you would use your software.

And only then

> write the code to make it happen.

When you first imagine yourself as the user of the code

