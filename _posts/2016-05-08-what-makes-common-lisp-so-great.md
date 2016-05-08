---
layout: post
title: "What makes Common Lisp so great a language?"
date: 2016-05-08T00:00:00
categories:
  - Programming
tags:
  - Opinion-based

---

I've recently come to a somewhat real use of Common Lisp for a project
I work on.  I've always been attracted to somewhat esoterical and
not-so-commonly-used languages, because they have always given me
insights on programming that are so hard to realize anywhere else.

You don't usually understand functional programming until you are
forced out of side effects, and you may not fully comprehend side
effects until you meet Haskell's Monads, or the importance of
referencial transparency until you write a functional language
compiler.

With Common Lisp, it's all about [homoiconicity][homoi] <sup>(from
Greek: homo=same + icon=representation)</sup>. Briefly, this refers to
the fact that in lisp dialects code is first-order.

Well, in Common Lisp there are so many first order things, but let's
start with code.

Homoiconicity
=============

As said before, code and data homoiconicity refer to the property that
they are represented the same. Both are represented in a syntax called Symbolic
expressions (S-expressions or sexps for short). Sexps are very simple
pieces of syntax: they are either symbols, or they are lists.

<ul>
<li>symbol</li>
<li>(sub-sexp1 sub-sexp2 ...)</li>
</ul>

So, the symbolic expression:

{% highlight common-lisp %}
(floor 5 2)
{% endhighlight %}

Is the list of three elements, which are the symbols `floor`, `5` and
`2`. That is data. But that data is also code.

Data as code
------------

If we evaluate that previous expression, it returns two values (because,
yes, Common Lisp can return multiple values, and that is not the same
as returning tuples), the first one being the division 5/2 and the
second one being the remainder.

{% highlight common-lisp %}
> (floor 5 2)
2
1
{% endhighlight %}


That list can be generated from the following code:

{% highlight common-lisp %}
> (list 'floor 5 2)
(floor 5 2)
{% endhighlight %}

That function, list, builds a list with the rest of the elements. As
CL is a call-by-value language, the quote in floor will avoid it being
evaluated as a variable.

However, entering the first value directly on the REPL (how the
interactive loop is called) will compute 5/2, so it is fundamentally code.

But lisp data can be turned into lisp code easily, via eval.

{% highlight common-lisp %}
(eval (list 'floor 5 2))
2
1
{% endhighlight %}

Not only that, lisp code can be manipulated as if it were lisp data,
by using macros.

> Macros in lisp have nothing to do with C macros.


Code as data
------------

That's the other part of the equation. Fortunately, it is a quite
symmetric relationship: code can be manipulated as if it were a
regular sexp.

How so? Well, in Common Lisp, when a file is loaded into the
evaluator, three stages happen:
<dl class="dl-horizontal">
<dt>read</dt>
<dd>The file is read into s-expressions, and several intricate things
may happen, due to something called reader-macros</dd>

<dt>compile</dt>
<dd>The read expressions are macroexpand-ed and compiled into a bytecode or object code for later evaluation</dd>
<dt>evaluation</dt>
<dd>The compiled expressions are evaluated</dd>
</dl>

In the first stage, the environment makes sense of the input stream
data (e.g., characters) as s-expressions, so here we can influence the
lexer (i.e., the reader algorithm) in order to extend our syntax
beyond s-expressions.

But it is the second stage the one that allows code to be manipulated
as data. As we already have read the file, we now have s-expressions
representing our code which are in their way to being compiled.

Thus, macros in lisp can be *semantic macros*, and not just *lexical
macros*, e.g., the textual substitutions that happen with the C
Preprocessor and similar.

Besides, the Common Lisp compiler is first-order itself, which means
that during compilation, **the full Common Lisp runtime is
accessible**. That means that regular functions can be called inside
macros, therefore a macro is a compile-time run function operating on
your program's AST, and which should return another AST for
compilation. Effectively, by using macros in Common Lisp, you are
writing *programs that write other programs at compile time*.

This macro mechanism is so flexible and extensible that the language
continuously encourages you to write your own
mini-DSLs<sup>Domain-Specific Language</sup> for every
problem. Because it is so easy and straightforward.


The meta level
--------------

The project I'm using Common Lisp with at the time involves writing
some code that can mean two different things, depending on how the
code is reached. As I can introduce definitions for a DSL on-demand, I
can give different semantics to my data by directly writing macros for
each case, instead of me manually interpreting the code. That is,
instead of writing my own two interpreters for the code, I leverage
the existing Common Lisp EVAL function, and I just introduce the
needed definitions into the EVAL environment in each case.


Why is Lisp still useful when we have X?
========================================

Well, consider the language features of your recent X.

Let's consider if it is up to par with a language designed in the late 1950s.

- Does it have a function type? (i.e., functions as first-class objects)
- recursion? (how about tail-call optimization?)
- garbage collection?
- programs composed of expressions? (i.e., no distinction with statements?)
- a symbol type? (symbols are not strings, and can be tested for
  equality by comparing pointers)
- a notation for code using trees for symbols?
- the whole language always available? (at read-time/compile-time/eval-time?)
- a multiple-inheritance multiple-dispatch class-based object oriented
  mechanism? (Common Lisp has the CLOS, or Common Lisp Object System)
- A MetaObject Protocol (MOP)?
- First-order continuations?
- Non-local exits?
- Constructors and destructors?
- dynamic variables?


I'm going to leave this post here, but if you have some time and
curiosity, give Common Lisp a look. I have yet to encounter a more
expressive language.



[homoi]: https://en.wikipedia.org/wiki/Homoiconicity
