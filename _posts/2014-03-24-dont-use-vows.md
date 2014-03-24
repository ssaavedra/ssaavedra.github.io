---
layout: post
title:  "Reasons you should never use vows"
date:   2014-03-24 13:01:00
categories: pfc
tags: javascript flame nodejs
use_more: true

---

Dealing with a dynamic scripting language is great at some point, for
prototyping and agile development; but, what about testing?

You will need a testing framework, a JUnit/TestNG of sorts if you come
from the Java world, and you want to rely on it for your testing,
i.e., you want the testing framework to *just work*. I'll tell you now
my experiences with VowsJS and how it failed miraculously in its duty
for me.

<!-- more -->

This hereby is my personal opinion and it may or may not represent the
absolute reality.

I'm making a project which contains a lot of JavaScript for the server
side, and I'm testing it on Node.js, because it's quite fast to do. Of
course, as a Computer Science literate, or sort of, I *know* tests are
meaningful, necessary and great for development itself, to test
regressions and all of your workings. Even subconsciously, tests are
great, because when you break something you may think "you've done
work as to interfere in your previous work", but if you don't you may
still think "your work is awesome: you didn't break anything".

Use whatever phrase it feels for congratulating yourself: you're
making progress.

But what happens when a test goes wrong?

Well, different frameworks are different worlds, but I'll tell you
what I think should happen.

First, the test should be reported failing, or erroring, depending on whether
assertions failed or unexpected stuff happened outside the testing
environment (such as an API change, or a DB connection problem if we
don't mock the storage layer when we're testing other stuff).

That sounds very obvious: the purporse of tests is to be known whether
they pass.

But second, and I think it's of most importance, the reporter **MUST**
tell you *what*'s failing, and *where*.

Even if they don't tell you how to fix it (ha, that would be awesome
BDD), knowing **what** fails is very important.

So, well, Vows (currently) fails so much at it: advertised as a
promise-friendly and asynchronous test framework, errors on async
calls are not reported, and if you manage to get some reporting, it
won't tell you where did the call come from (which is absolute
nonsense if you have more than, let's say, two tests).

I spent so much time actually *debugging* my test framework that it
should just not happen. A testing framework is supposed to be a tool
for others to use (when it's ready), and that's why I changed.

I didn't research too much into it, but I went to mocha with chai and
chai-as-promised to deal with promises in BDD-style, and I find it
quite comfortable.

Migrating my test battery took me like 15-20 minutes, most of which
was making more assertions and in a better way, and a bit of changing
the structure and then it just worked great.

Besides, I like a lot more Chai's "expect" BDD idiom; I find it quite
clean, expressive, and harmless.

An assertion in this style is set as follows:

    var something = null;
	expect (something).to.be.null;

	something = 5;
	expect (something).to.equal (5);
	expect (something).to.be.typeof ('number');
	expect (new Error).to.be.instanceof (Error);

And (with chai-as-promised) it works great with promises, such as:

	var promise = when (42);

	// Assert that the promise is not rejected
	expect (promise).to.be.eventually.fulfilled;

	// Assert things about the result given in the promise
	expect (promise).to.be.eventually.typeof ('number');
	expect (promise).to.eventually.equal (42);

	var promise = when ([]);
	expect (promise).to.eventually.respondTo ('slice');

	// Assert properties about objects created from a specified constructor
	expect (Object).to.have.property ('prototype');

    // Use on constructors themselves, not on created objects:
	expect (Object).itself.to.respondTo ('create');

Don't you find it expressive?

There's another flavour, called "should" which would go like this:

	something = [];

	something.should.be.an.instanceof (Array);
	something.should.be.empty;

	// And so on

But I like that approach less because it pollutes the global namespace
(making should a property of everything), and I am enlisted against
the Global-Namespace Warming, for which the Global-Namespace Pollution
and their greenhouse effect are quite to blame :)

Anyway, I still think that polluting the namespaces in the tests is
still less troublesome than using Vows. But yeah, there might be
caveats, and that's why you should use expect instead ;)

End of the rant.

I really hope (seriously) for these words to be nonsense on some
day. I hope vows gets more mature, bugs cleaned and features provided
so that it can be used by people in my situation, who have not enough
time to deal with the project plus the test framework at the same
time; but currently this is how I felt when I tried to use it
properly, and asynchronous.


Cheers!
