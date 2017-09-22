---
layout: post
title: Making ReelValley - Overview
---

Recently the team which I am proud to be the part of released a new game named
[Reel Valley](https://apps.facebook.com/reelvalley). The game is done in almost
100% pure [Nim](https://nim-lang.org) and I've received a lot of questions
regarding implementation details and decisions taken. Questions that inspired me
to write this first article in my blog :).

Some History
============
The development started in late 2015 and that was the time of many
strategic decisions.

Technology stack
----------------
First thing that was obvious - the game had to run in web browsers as
well as on mobile devices. All of such targets imply that the technology stack
should provide minimal runtime overhead even for small casual games. It also
was obvious that the codebase had to be almost the same for all the targets.
Given those facts it is clear that whichever language is used it should be
ahead-of-time compilable to native code and to JavaScript. 2 years ago the
Asm.js target was pretty unstable and experimental on the market of game
engines, so the chances to use third party engines (especially closed source ones,
that we could not hotfix if needed) reduced.
The idea of what the game will look like was rather simple to assume that we
are able to make our own engine. Looking back now, its funny to realize how
naive we were :).

Being a person who never stops seeking for better technologies (that is how I
like to think of myself ;) ), I've been searching for a better language than C++
for a very long time. After learning and trying lots of the well-known languages
and no satisfaction I've switched to less mainstream ones. Among them were
[D](https://dlang.org), [Rust](https://www.rust-lang.org),
[Crystal](https://crystal-lang.org), some others, and [Nim](https://crystal-lang.org),
which I've suddenly discovered on Github. I shall not describe all of the
pros and cons here, just those that were the key factors in our final decision.

Nim language design is pretty fresh, concise and clean from the
semantics standpoint, and what I haven't seen in other languages, its semantics
allows for highly performant code. The performance is on par with C, while the
code looks as simple as Python (even simpler, because in Nim you can see type
names where appropriate to not make false type assumptions like in scripting
languages :) ). Nim provides metaprogramming capabilities on par with Lisp
and syntax extension capabilities on par with... TBH I don't know any language
that would match here. Nim compiles its code to C, C++, and JavaScript, which
covered our needs, so all those features looked really sweet. However like with
all new technologies there were scary risk factors that had to be evaluated
very thoroughly.

Compiler bugs. Lots of bugs that either needed to be worked around or fixed.
What if the core maintainers will not fix those for us timely? How hard would
it be to fix those ourselves. And if there are some bugs that can't be fixed
in a timely manner, how hard would it be to workaround those? And Nim has
successfully passed that evaluation. 2 simple reasons: 1. Nim compiler codebase
is rather concise and comprehendable for the most part, despite highly
sophisticated stuff it does. Overtime we have contributed quite a bunch of fixes
and features. And 2. Nim allows you inline target backend code (be it C or
JavaScript) right into the Nim source code. So we are always confident that
neither of Nim bugs shall prevent us from getting to the point, we always can
fallback to C where needed.

Community and available third-party packages. What if there's no package available
in Nim that we suddenly need, while in a mainstream language you can easily find
dozens of libraries that do the same thing. Can we implement the needed packages
on our own, and if not, how easy it is to bind to a lib from a foreign ecosystem?
And the answer is YES again. 1. Productivity that the language offers is
extremely high, the needed parts can be reinvented much faster than, say in C++
or a typeless scripting language. And 2. It turned out to be very easy to bind
to existing C/C++/JS code, be it in a static lib, dynamic lib, C header file,
or just copypaste the needed code and inline it, no strings attached.

So after all risks considered, we have started, there were huge wins and small
losses, and I still think that the choise was right, and I'm not leaving Nim
anytime soon. It is simply brilliant.

Techincal Insights of the Game
==============================
For those interested in what libraries we are using. There are around 60 direct
and indirect dependencies in the client and around 20 dependencies in the server.
Thankfully, nimble package manager installs those with a single command. And
[Travis](https://travis-ci.org) makes sure that at least our packages are working
properly with the actively developed ecosystem.
The core of the game is [nimx](https://github.com/yglukhov/nimx) and [rod](https://github.com/yglukhov/rod).
There are around 15 other libraries that we developed along the run, such as
[jsbind](https://github.com/yglukhov/jsbind), [preferences](https://github.com/yglukhov/preferences),
[nimongo](https://github.com/SSPKrolik/nimongo), [nimsl](https://github.com/yglukhov/nimsl),
and others.

I've been asked a question about Nim JS backend, whether it is good or not. And
the answer is yes, it is pretty darn good (especially with
[closure compiler](https://github.com/yglukhov/closure_compiler), and we're using
it for our tools, but not the game itself. At some point we had to switch from JS to
[Emscripten](https://emscripten.org), just because Asm.js is much faster, and
thanks to Nim the transition went pretty quickly. Hopefully we shall soon support
WebAsm target as well.

To Conclude
===========
As it is likely clear from the post Nim turned out to be a great language for
any kind of software development, be it commercial production grade software,
or a couple-of-lines one shot script, not to mention the huge amount
of fun you get when using it. It is very easy to do early prototypes. As you may
know prototyping involves chaotic moving of chunks of code around, copy-pasting,
changing the semantics of your types (e.g. I though it should be immutable, but
now it looks like it has to be mutable), so at the prototyping stage you want to
use some low friction language like, say Python or JS. And Nim also falls into
this category. No need to fight the compiler to prove anything. But when the
prototyping is done, the code is cleaned, you're done. No need to port it to
lower level language because everything you can optimize in C, you can do in Nim.

So hopefully I have answered some of the main questions. If not, ping me on the #nim
IRC channel, or [Twitter](https://twitter.com/yglukhov), will be happy to elaborate,
and maybe even post more articles here, who knows :).
