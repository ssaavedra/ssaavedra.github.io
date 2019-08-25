---
layout: post
title: "What is wrong with (economics in) FLOSS?"
language: en
categories:
  - Politics
  - Philosophy
tags:
  - FOSS
  - Economy
---

There is currently an interesting debate on the JavaScript community,
centered around the `standard/standard` project on GitHub. Some people
are interested on the economics of free software and trying to explore
different avenues for getting back money for contributing to open
source.

You can see the full thread
[here](https://github.com/feross/funding/issues/10), and I invite you
to read my thoughts on the issue (which I have replied in the original
thread, and may later see comments on it).

----

Forefront: if you are currently maintaining a FLOSS project and feel
*entitled* to someone paying you for doing volunteer work, I humbly
invite you to revise your thoughts, or otherwise step down and leave
the FLOSS community. We'll do fine without you. Yeah, maybe you can
wreak havoc for a couple of versions on some dependencies and create a
bit of history of "what shouldn't happen", but that will be it. We'll
fork your stuff, fix it and move on.

But then, there are those that, without feeling entitled to it, may
think that *it would be nice if* Free Software projects could have
people working full-time on them in order to increase their value over
proprietary alternatives. And here, I do sympathise with you,
*completely*.

First of all, I think there's a problem with the discourse between
Free Software and Proprietary Software at a fundamental level if we
can even think that only in general proprietary software can be a
source of sustenance/income, a thought some folks here are seemingly
supportive of.

While I don't think that Free Software developers are entitled to a
sustained income, I don't disagree on the general point that such
contributions can be valuable and should be fairily rewarded, and
encouraged to continue participating with more valuable contributions,
and thus, achieve such an income.

In particular, if we want to create *more* high-quality free software
to, essentially, compete against proprietary software for the freedoms
and guarantees it gives us, then, somehow, in an abstract way, the
Free Software Movement is competing for labor against for-profit
corporations. This doesn't necessarily mean that all free software
should be monetized, and of course it does not say anything about how
and from who should that money come from. I do agree with
@traverseda's point on value exchange: it's tricky, and even more when
we talk about something as intrincate as software, usually built from
thousands of dependent pieces whose contribution to the whole is never
easily calculated.

The whole idea of Free Software and Open Source is an exercise on
models about shared capital, and shared property (intellectual
property, which is itself an amusing construct that we came up with to
try to monetize off the press machine, since the cost of book-copying
became obsolete at that point). This shared property, "the Commons",
is also infinitely redistributable at marginal costs, it's not like a
"community fund" where if take money for community expenses, it leaves
the fund. Here, it stays for the rest of the community to also use it
again in as many ways as imaginable.

The thing is that, while all the intellectual works of a project are
infinitely redistributable, the time of developers/maintainers
committing their time to this "community fund" is definitely not. But
then, what we are asking is *how can we fairily set a value for the
work that the "intellectual workers" did, and who should pay that
value?* In my opinion, the answer to this question has never been
properly formulated throughout History.

We have many mechanisms, like grants, patronage, royalties,
patents..., but (again, in my opinion) all of them have some or other
nuances that make them unfit in different circumstances.

Cross-linking from
https://github.com/feross/funding/issues/11#issuecomment-524609870,
you can find most of these approaches applied to software on the
[lemonade stand](https://github.com/nayafia/lemonade-stand) repo.

Personally, I'm fond of crowd-sourced bounties and encouraging users
to pay consultants to develop the features they'd need (as they would
pay on proprietary software). It has some frictions, and it's not an
easy encouragement (that's why I'm mentioning it as part of *what's
wrong right now*), and the conflict may arise as to whether some
candidate implementation actually fulfills what's being asked, but by
pooling money together on an issue you can get the attention of
someone who may see value in implementing it.

- It gives users a loud voice on what's important to them by voting
  with the same thing that gives them value
- It still gives a voice to those who can commit less money, because
  if the thing is relevant, it will be bountied further by others who
  may see the feature interesting too
- It is more transparent about who the money goes to, and what for
- It gives users an incentive to become contributors, and increase the
  long-term strength of the project, also giving the ability to core
  developers to step back if they wish to, without feeling they are
  irreplaceable

However,
- This has the risk of money taking control of the planning and
  long-term development of the project; but there the core developers
  may also decide if money provides more value to them than actually
  setting a long-term plan *pro bono*.
- Having pledges linked to the "upstream" project prevents them from
  being vetted by the backers until upstream maintainers can review
  the task, reducing the value capture of forking the project and
  implementing missing features (e.g., on an unmaintained repo with a
  maintained fork)

There is also the problem of reviewing/merging and how that value
should be split, but before considering that, we should also put in
the other hand if, instead of reviewing the changes the project was
split into a fork, the loss of value created by the fragmentation of
the ecosystem, which should not be dismissed either.

Having bounties for performing work (or recurring patronage) gives
back to the contributors the *labor-value* of their contributions (at
least, it does if the contributors accept bounties according to the
effort it requires for them), independently of the actual *use-value*
of the project.

However, the neoclassical-economy-aligned who read this that reject a
labor theory of value may not find the former argumentation
satisfactory, although I think they could find some alternate
argumentation under their framework.

These arguments probably work only for truly community efforts; I
haven't yet matured the thoughts enough to understand the implications
on cooperating for-profit companies on open-source projects, although
their current model doesn't seem too problematic for them.

On a side note, if you are a current maintainer of a project, and you
think you don't have the time (or money to avoid the need to rent your
time) to continue contributing to a project, *maybe* you can also
**just step back**, and advertise a *contributors wanted* notice, so
that others understand the situation and pitch in. In the end,
collaborating should be a gratifying business, and if it is not for
you, maybe you can leave it to others who find it so and come back
later.
