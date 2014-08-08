---
layout: post
title:  "How to deal with amdefine if something breaks"
date:   2014-08-07 19:01:00
categories: pfc
tags: javascript nodejs
use_more: true

---

In JavaScript there are many ways of loading modules. But not taking
care of small details on how the JS standard is written can create
funny side effects that will prevent your app from running. And you
can fix the situation, but you may not know how you did it.

This post is about how to work with `amdefine` and how a module can
break your other hybrid browser/node-require nicely-created packages.

<!-- more -->

Hi again. It's been awhile since my last update. I've been working on
the same project for the time being (coalesced with other work to do),
and I've come to use lots of JavaScript tools. Many of them I didn't
hear about until this year. The JS community is getting crazy. And
some of them aren't properly crazy, but let's let it be that
way. Today's post is about how to solve an issue, not a rant :)

As some of you, in the JavaScript world, may know, there are lots of
ways of loading and requiring modules within your
applications. Node.js has its own, and other standards were created to
allow more flexibility and to port that practice to the browser, where
most of JavaScript presence happens still, anyway.

Having package loaders is great, but having different standards for
loading is not so much. Fortunately, node's way and AMD (short of
*Asynchronous Module Definition*) are not incompatible.

In fact, many packages distributed through npm nowadays offer support
for both loading mechanisms. Although AMD was developed for the
browser, there are implementations to use it in Node, most
prominently, the `amdefine` package.

However, using AMD for your modules would require amdefine as a
dependency on your package, which is actually not needed if you can
provide both interfaces. And that's actually quite simple.

That's what modules such as cujojs's `when` Promises/A+ implementation
does, precisely. In short, let's assume you were developing
node-style. The following could be a module.

{% highlight javascript %}
var code() { var fs = require('fs'); };
/* more code */
module.exports = { spaghetti: code };
{% endhighlight %}

Well, this module could be written in AMD style, this way:

{% highlight javascript %}
if (typeof define !== 'function') {
	var define = require('amdefine')(module);
}

define(function(require) {
	var code() { var fs = require('fs'); };
	/* more code */
	return { spaghetti: code };
});
{% endhighlight %}

Easy enough. In short, you wrap all of your code inside a closure,
which gets as an argument your  usual "require" function.

In order to work, you must ensure that you are loading the amdefine
module loader before, with the if clause provided in the beginning of
the code. This must be present in all your files, and this way, your
code **does** depend on amdefine to work.

However, if you wanted to be flexible in terms of the AMD loader, or
any module loader that you wish to work with, you could wrap your
code, instead, with a function such as the following.

{% highlight javascript %}
(function (define) {
define(function (require) {
	var code() { var fs = require('fs'); };
	/* more code */
	return { spaghetti: code };
});
})(typeof define === 'function' && define.amd
	? define
	: function (factory) {
		module.exports = factory (require);
	});
{% endhighlight %}

Ok, what this does is immediately on requiring of the module, the
latest line is evaluated, and if *define* is already a function, then
we can assume that an AMD-style loader is in place and we can pass on
define to our closure, and if it's not, we can place ad-hoc our own
loader for node: just making our *define* function to be a wrapper so
that the result of the callback we get (on `define(callback)`) is
assigned to `module.exports`, and thus, exported.

Seems easy, huh?

Well, there's a caveat with this approach.

You must trust all your modules **not to pollute** the *Globals*
namespace.

Yeah, that should never happen. But sometimes it does. There are
malformed packages, and due to the way some things in JavaScript are
standarized, it may not be completely trivial to know what happened.

Now that I have introduced you to how stuff works, let's expose the
problem I faced.

I was using an Inversion-of-Control (IOC) module, called `inverted`,
which in turn uses amdefine, because this way it works both in the
browser and server. However, there is a typo in the latest published
version (0.2.4 at the time of this writing, although I've already sent
a pull-request for a fix) in the first conditional, that you have just
read a few paragraphs above.

I will reproduce the above code, and then I'll reproduce inverted's
conditional.

{% highlight javascript %}
// Sample amdefine code
if (typeof define !== 'function') {
	var define = require('amdefine')(module);
}
{% endhighlight %}

{% highlight javascript %}
// inverted's conditional
if (typeof define !== 'function') {define = require('amdefine')(module);}
{% endhighlight %}

You may have required several attempts to find the missing `var`
before `define =`.

Does this affect us?

You may imagine that it doesn't because node must have a way of
capturing all non-declared vars before polluting the global namespace
between modules... but it doesn't.

The author of this --and any other modules-- could do (and should)
have written their code as **strict**, by using the 'use strict'
pragma at the beginning of the file. This would have told them that a
variable was not being defined and it would crash their program. But
people does not still use strict mode as extensively as we might
want. And there are cross-browser compatibility issues with strict
mode, and if you want to support older browsers, well, it's all a
mess.

Anyway, this little problem would deal no harm on other modules that
**require** amdefine's behaviour, and that use the second form of
writing modules that we introduced earlier in the post. However, if
you use code that is adapted to both cases of the story, and as node
is not the browser and AMD has no way of supporting a double
definition in the same module, it will break with an error saying
**amdefine with no module ID cannot be called more than once per
file** will appear, because we will be referring in the next files to
this old define.

That is, you **cannot**, in the same file, do this:

{% highlight javascript %}
if (typeof define !== 'function') {var define=require('amdefine')(module);}
define(function () { return { magic: 'code' }; });
define(function () { return { nonmagic: null };});
{% endhighlight %}

That is because *in node*, you declare and define the `define`
function **in each module**, independently. You don't reuse
define. That's why node modules, if they want to be AMD compliant,
they must have this test. Because amdefine can only know *who* called
him by giving it the current `module` instance. The `module` variable
in Node contains information such as the filename loaded, which
amdefine uses to plug the appropriate loaded code in the required
statements; that is, when you do `define(function (require) {a =
require('./smth'); /* more code */ })`.

I first avoided this issue by using an experimental feature in
amdefine, called `amdefine/interceptor`.

The interceptor mangles Node's require hook for JavaScript files, so
that all of them are prepended as the first line the correct
conditional written above on this post. That is great news, because
you don't have to do it in every file; but if you do it manually in
every file it will work anyway. This is just a faster lane.

However, you should not touch other people's modules, or you won't be
able to deploy your work safely with npm, and then you'd have a lot of
pain for maintenance. So, in order to save kittens from being killed
whilst doing maintenance later, it's clear we should not touch other
people's modules --unless we fork them, carefully.

Then, I would like to propose the reader what would happen if after
loading inverted one was to load a module which used the third
approach?

I'd rather remind here that the third, hybrid approach does not have a
defining if.

The answer is that it would break. Define would be introduced before,
and it would have an outdated module definition, so the later
`define(cb)` would fail because it has already been called before.

We already knew it because I told you a while earlier.

But then, a more interesting question is **why does it not fail on AMD
modules**?

There is a conditional stating that `if (typeof define !== "function")
{...}`. As per hybrid modules, we *know* that in that particular
moment, `define` should be a function. But the sentence inside the if
clause gets executed shamelessly anyway.

How?

I'd like it to be an exercise to the reader, but here's the answer
anyway. If you'd like to think about it, don't continue reading yet.

It happens that in JavaScript, the `if` clause *does not* create a
scope. So all *var* declarations *actually happen* in the same
scope. As *var* declarations in JavaScript are independent of position
(that is, all vars could be declared at the beginning of their
respective scope and produce an equivalent program, the previous
conditional is equivalent to:

{% highlight javascript %}
var define;
if (typeof define !== "function") { ... };
{% endhighlight %}

And that magically performs two things:

If define was already a var, declared within the same scope, then
nothing happens: var declarations are idempotent.

However, if define was a global variable, declaring a var in this
restricted scope will shadow the global variable. The new var has been
declared but not yet defined, so it has type (and value) of
`undefined`, which is definitely not *function*. Thus, the code
execution gets inside the conditional clause and requires the module
loader, as expected.

I hope this can enlighten your JavaScript-fu.

