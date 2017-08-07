Advanced FP Course
=====================

You
---

* Have completed, or are capable of completing, the [Data61 FP Course](https://github.com/data61/fp-course)
* Have a few months self-study to your name
* Want to know how to build larger applications with statically typed FP
* Are willing to accept that a web application is a sufficient choice

We
--

* Have constructed a sequence of goals of increasing difficulty
* Have provided a framework within which to apply these goals
* Have included relevant components of larger applications:
  - Package dependencies
  - Project configuration
  - Application testing & building
  - Encoding / Decoding messages (JSON & Binary)
  - Persistent storage integration
  - App state & configuration management
  - Error handling & reporting
* Will utilise both type & test driven development techniques
* Will explain architectural and design trade-offs when appropriate

UNSURE:
===

Use of lenses and classy `mtl` will be left to supplemental material. I would
like to be able to use it lenses more but the additional explanation required
might be a bit much and not very predictable ?

Ultimate Goal
===============

This course aims to teach some of the techniques for building a larger
application with FP, using Haskell. By the end of this course you should be
comfortable tackling more advanced projects, and expanding on the concepts and
choices presented here in your own future efforts.

We will build an ultra basic web application and build upon it.

Subgoals (?)
========
- ghcid
- hedgehog
- cabal files

Goals
======

1 - Death to Strings
---
Start up and Servant introduction.

* Use Scotty/Warp for the "hello, world" application.
* Explain that we're using strings for routes and this is bad(TM).
* Move to Servant, explain why, show type driven dev to explain routes->function relationship.

2 - Faking Global Vars with Science
---
Show that nothing can be changed in the current app without recompliation.
Rework the application so we can change the port values and have general app
config.

* Read a file in app start up to allow for port config changes.
* Explain that this can be cumbersome to explicitly pass the config around, there are better ways(TM).
* Add `mtl` dependency.
* Natural Transformation required from our new `ReaderT` to Servant (too much?).

3 - Would you like to play a game?
---
Introduce handling input/output also preempt the inclusion of persistent storage.

* Add rock-paper-scissors data type. (Don't create Enc/Dec instances yet)
* Add route to accept an RPS move that always plays paper against the user.
* Go over what the errors from Servant & GHC are telling us.
* Add the required instances.
* Change the function up so that it randomly selects a move, evaluates victory/defeat.

4 - Type safe tantrums
---
Introduce error handling by breaking REST rules by having our application throw
an error when it loses at Rock-Paper-Scissors.

* Change RPS function to throw an error on defeat, creating required error data types.
* Discuss the errors that appear RE return values and making Servant respond appropriately.
* Introduce `ExceptT`, change the `ReaderT` from earlier to what we want it to be.
  * More discussion to be had here regarding the ordering of transformer stacks.
* Discuss the errors that appear. Work through fixing these with type-holes in the Natural Transform.

5 - Elephants
---
We'd like to be able to store a history of RPS games. 

* Create a datatype that has some game info, respective moves, outcome, time.
* Introduce and integrate `postgresql-simple`:
  * Config for connection
  * `Connection` on the `ReaderT`
  * Using `Connection` from `ReaderT` in `query` function(s)
* Add the required instances so we can read/write our datatype (provide a SQL file to initialise/purify the DB)
* Add our route to the API.
* Work through the errors using type-holes to create our function.
* Stringly queries acceptable for now - postgresql-simple

** Prompt discussion about what lurks beneath the surface of the `IO a` query functionality.

6 - Except when exceptionally excepted
---
Handling, catching, and rethrowing exceptions. Motivate errors as values over exceptions.

* Possible ways postgresql-simple can fail on us, note that the types provide no information
* Introduce exception handling with `catching` and `handling` to our error values.
* Generalise our current DB querying technique so that we take a query and
  handle some exceptional cases.
* Discuss the relevance of handling these errors:
  * Shouldn't they just come up in testing?
  * I'm sure there are other points here, some Haskell applications are built
    not to care about these sorts of errors.
  
* Logging ?
