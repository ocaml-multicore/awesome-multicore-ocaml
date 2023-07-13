# Awesome Multicore OCaml

A collection of libraries, experiments and ideas relating to OCaml 5 (multicore + effects). With the [merging](https://github.com/ocaml/ocaml/pull/10831#issuecomment-1008935795) of the main `multicore-ocaml` branch into OCaml `trunk`, a parallel and effectful world is fast approaching.

This repository collects useful libraries, ideas and experiments relating to the use of Multicore OCaml features (primarily parallelism with domains and direct-style code with effects).

For contributing, [see the guide](https://github.com/patricoferris/awesome-multicore-ocaml/blob/main/CONTRIBUTING.md).

If you are wondering what even is Multicore OCaml, you could start by watching [KC's keynote at ICFP 2022](https://www.youtube.com/watch?v=zJ4G0TKwzVc&t=2s). To know more about how Multicore OCaml evolved over the years, see [this video from 2014 about Multicore OCaml by Stephen Dolan and co.](https://watch.ocaml.org/videos/watch/490b5363-01b6-45d8-9b7e-c883a20026a1), then [one on how to parallelise your OCaml code](https://watch.ocaml.org/videos/watch/ce20839e-4bfc-4d74-925b-485a6b052ddf) and [how we can adapt the existing ecosystem to support Multicore OCaml](https://watch.ocaml.org/videos/watch/629b89a8-bbd5-490d-98b0-d0c740912b02).

For understanding effects in OCaml 5 you can watch [KC Sivaramakrishnan's Effective Programming in OCaml 5 talk](https://www.youtube.com/watch?v=xKUzN-McZUk) and also Thomas Leonard's talk at the [2021 OCaml Workshop](https://v3.ocaml.org/workshops/ocaml-workshop-2021) on [Experiences with Effects](https://watch.ocaml.org/videos/watch/74ece0a8-380f-4e2a-bef5-c6bb9092be89). Note both videos use a syntax for effects that will not exist in OCaml 5.00 (see also [ppx_effects](#ppx_effects)).

For more resources like this, check the [Multicore OCaml wiki](#wiki).

*There's a WIP PR to start submoduling repositories so it can be a one-stop shop for taking OCaml 5.00 for a spin, see [the PR](https://github.com/patricoferris/awesome-multicore-ocaml/pull/1)*.

## Table of Contents
- [Installation](#installation)
- [Libraries](#libraries)
  - [Eio](#eio)
  - [Affect](#affect)
  - [Domainslib](#domainslib)
  - [Lockfree](#lockfree)
  - [Lwt Support](#lwt-support)
  - [Async Support](#async-support)
  - [Parany](#parany)
  - [Processor](#ocaml-processor)
  - [cohttp-eio](#cohttp-eio)
  - [mirage-crypto-rng-eio](#mirage-crypto-rng-eio)
  - [Js_of_ocaml support](#js_of_ocaml)
  - [ocaml-tls](#ocaml-tls)
- [Testing](#testing)
  - [multicoretests](#multicoretests)
- [Tooling](#tooling)
  - [Olly](#olly)
  - [Eio-console](#eio-console)
  - [TSan](#tsan)
- [Experiments](#experiments)
  - [Dream](#dream)
  - [Ppx_effects](#ppx_effects)
  - [Gemini Protocol](#gemini-protocol)
  - [Multi-shot Continuations](#multi-shot-continuations)
  - [Mirage network](#mirage-networking-experiments)
  - [Capnp-rpc](#capnp-rpc)
  - [gRPC](#grpc)
- [Ideas](#ideas)
  - [Non-blocking Codec](#non-blocking-codec)
- [Resources](#resources)
    - [Wiki](#wiki)
    - [Discuss Threads](#discuss-threads)
    - [Monthlies](#monthlies)
    - [Benchmarks](#benchmarks)

## Installation

OCaml 5 is out! The compiler can be obtained with the following instructions on Linux and Mac machines.

```
λ opam update
λ opam switch create 5.0.0
λ eval $(opam env)
```

## Libraries

### Eio

Repository: https://github.com/ocaml-multicore/eio

Eio implements an effects-based direct-style IO stack for multicore OCaml. It supports multiple backends including a performant [io-uring](https://unixism.net/loti/what_is_io_uring.html) one and a portable [libuv](http://docs.libuv.org/en/v1.x/) backend. A [0.1 release](https://github.com/ocaml/opam-repository/pull/20695) is now available from opam-repository.

### Affect

Repository: https://erratique.ch/software/affect

> Affect provides composable concurrency primitives for OCaml using the effect handlers available in OCaml 5.0. Affect should be seen as an experiment at that point.

### Domainslib

Repository: https://github.com/ocaml-multicore/domainslib

Domainslib provides data-structures for parallel programming on top of multicore primitives. This includes a work-stealing task pool and channels for inter-domain communication.

### Lockfree

Repository: https://github.com/ocaml-multicore/lockfree

Lock-free is collection of [non-blocking](https://en.wikipedia.org/wiki/Non-blocking_algorithm) data structures for multicore OCaml.

### Lwt Support

Some useful and relevant OCaml.5 x Lwt discussions:

 - [Is mixing lwt and multicore safe?](https://discuss.ocaml.org/t/is-mixing-lwt-and-multicore-safe/8714)
 - [Multicore, Async and Lwt](https://discuss.ocaml.org/t/multicore-async-and-lwt/2687)

#### Lwt_eio

Repository: https://github.com/talex5/lwt_eio

`Lwt_eio` provides an "lwt engine" using [Eio](#eio) making it possible to use both Eio and Lwt in the same codebase. A [0.1 release](https://github.com/ocaml/opam-repository/pull/20701) is now available from opam-repository.

#### Lwt_domain

Repository: https://github.com/ocsigen/lwt

Lwt now has a package called [`lwt-domain`](https://github.com/ocsigen/lwt/tree/master/src/domain). This provides useful functions for programming with Lwt and [Domainslib](#domainslib).

### Async Support

#### Async_eio

Repository: https://github.com/talex5/async_eio

Async_eio allows running Async and Eio code together in a single domain. It allows converting existing code to Eio incrementally.

### Parany

Repository: https://github.com/UnixJunkie/parany/tree/domains

The Parany git branch called "domains" is compatible with multicore-ocaml.

### OCaml Processor

Repository: https://github.com/haesbaert/ocaml-processor

The library allows you to query the processor topology as well as set the processor affinity for the current process.

### cohttp-eio

Repository: https://github.com/mirage/ocaml-cohttp/tree/master/cohttp-eio

`cohttp-eio` is a HTTP client and server library built using `eio`. It enables monad-less, direct style programming of multi-core capable HTTP web server and client applications. Monads be gone. 

### mirage-crypto-rng-eio

Repository: https://github.com/mirage/mirage-crypto/tree/main/rng/eio

`mirage-crypto-rng-eio` allows to use various crypto functions in an `eio` application.

### Js_of_ocaml

Repository: https://github.com/ocsigen/js_of_ocaml

`js_of_ocaml` supports effects through CPS transformations - this means one can start running effects in browsers! Implementation details can be found in [Jérôme Vouillon
's PR](https://github.com/ocsigen/js_of_ocaml/pull/1340)

### ocaml-tls

Repository: https://github.com/mirleft/ocaml-tls

`tls_eio` supports effectful operations using Eio.

## Testing

### Multicoretests

Repository: https://github.com/jmid/multicoretests

Experimental property-based tests of (parts of) the OCaml multicore compiler. The project contains a randomized testsuite of OCaml 5.0 based on QCheck and two reusable testing libraries `Lin` and `STM`.

## Tooling

### Olly

Repository: https://github.com/sadiqj/runtime_events_tools

A collection of observability tools around the runtime events tracing system introduced in OCaml 5.0.

### Eio-console

Repository: https://github.com/patricoferris/eio-console

Eio-console provides an application for monitoring running programs. This works in the browser, communicating information over a websocket.

### TSan

Repository: https://github.com/OlivierNicole/ocaml-tsan

ThreadSanitizer (TSan) is an effective approach to locate data races in parallel code. This extended version of the OCaml compiler generates instrumented executables that will print error reports if a data race is detected.

## Experiments

### Dream

Repository: https://github.com/talex5/dream/tree/eio

See also the [draft PR](https://github.com/aantron/dream/pull/194) that uses [Dream](https://github.com/aantron/dream), Eio and [lwt_eio](#lwt-eio) to provide a direct-style interface to Dream.

### Ppx_effects

Repository: https://github.com/CraigFe/ppx_effects

The effects in OCaml 5 will not support a dedicated syntax, but plans exist for this perhaps once the effects are typed. Until then this ppx provides an approximation to the proposed syntax.

### Gemini Protocol

Repository: https://gitlab.com/talex5/gemini-eio

The [gemini protocol](https://gemini.circumlunar.space/docs/faq.gmi) implemented in direct-style using [eio](#eio).

### Multi-shot Continuations

Repository: https://github.com/dhil/ocaml-multicont

Built on top of the continuations in Multicore OCaml, `ocaml-multicont` provides a library for using continuations that can be applied more than once. See also [this discussion on discuss](https://discuss.ocaml.org/t/multi-shot-continuations-gone-forever/9072).

### Mirage networking experiments

Repository: https://github.com/TheLortex/networking-experiments

Goal is to port Mirage's TCP/IP stack on top of OCaml 5's effects for more manageable async code.

### capnp-rpc

Cap'n Proto is a capability-based RPC system with bindings for many languages. The [Eio port](https://github.com/mirage/capnp-rpc/pull/256) switches capnp-rpc from Lwt to Eio.

### gRPC

gRPC is a high performance, open source, general RPC framework that puts mobile and HTTP/2 first. The [Eio port](https://github.com/dialohq/ocaml-grpc/) adds native Eio support for OCaml gRPC.


## Ideas

Even less further on than [experiments](#experiments), the *ideas* section contains projects that would benefit from the features of Multicore OCaml.

### Non-blocking Codec

Repository: https://github.com/dbuenzli/nbcodec

Many libraries and tools written in the nbcodec style would benefit from continuations given by effect handlers in Multicore OCaml, including libraries like [jsonm](https://github.com/dbuenzli/jsonm) or [xmlm](https://github.com/dbuenzli/xmlm) perhaps.

## Resources

### Wiki

The [Multicore OCaml Wiki](https://github.com/ocaml-multicore/ocaml-multicore/wiki) contains a lot of useful information from design decisions to examples. Two notable repositories are: 

  - [Effects examples](https://github.com/ocaml-multicore/effects-examples): a collection of great examples with effects including cooperative threading and generators.
  - [Parallel Programming in Multicore OCaml](https://github.com/ocaml-multicore/parallel-programming-in-multicore-ocaml): a very good introduction to programming parallel programs in Multicore OCaml, including [using Domainslib](https://github.com/ocaml-multicore/parallel-programming-in-multicore-ocaml#domainslib).

### Papers

- [Retrofitting Effect Handlers onto OCaml](https://kcsrk.info/papers/retro-concurrency_pldi_21.pdf), PLDI, June 2021: Describes the design and evaluation of a full-fledged efficient implementation of effect handlers for OCaml.
- [Parallelising your OCaml code with Multicore OCaml](https://github.com/ocaml-multicore/multicore-talks/blob/master/ocaml2020-workshop-parallel/multicore-ocaml20.pdf), OCaml Workshop, August 2020: A tour on developing parallel programs with Multicore OCaml.
- [Retrofitting Parallelism onto OCaml](https://kcsrk.info/papers/retro-parallel_icfp_20.pdf), ICFP, August 2020: Deep dive into Multicore GC design. 
- [Multicore OCaml memory model](http://kcsrk.info/papers/pldi18-memory.pdf), PLDI, June 2018: Describes the memory model for multicore OCaml programs. This memory model is much simpler than existing language memory models such as Java or C/C++11 without sacrificing performance. 
- [Concurrent System Programming with Effect Handlers](http://kcsrk.info/papers/system_effects_feb_18.pdf), TFP, February 2018: Using effect handler for writing concurrent programs that interacts with the operating system.
- [Multicore OCaml](https://ocaml.org/meetings/ocaml/2014/ocaml2014_1.pdf), OCaml workshop, September 2014.

### Talks

* Introduction to Eio, Thomas Leonard, February 2023. ([video](https://watch.ocaml.org/w/1k1T919WGXoT4tjnRZEmMd))
* [k-CAS for sweat-free concurrent programming](https://gist.github.com/polytypic/3214389ad69b16d28b957ced86e1b1a4#k-cas-for-sweat-free-concurrent-programming), Vesa Karvonen, February 2023. ([video](https://watch.ocaml.org/w/nmtxFx5zDJLbEXRhraf3JU))
* Retrofitting Concurrency – Lessons from the Engine Room, KC Sivaramakrishnana, September 2022. ([video](https://www.youtube.com/watch?v=zJ4G0TKwzVc&t))
* [Experiences with Effects in OCaml](https://icfp21.sigplan.org/details/ocaml-2021-papers/16/Experiences-with-Effects), Thomas Leonard, August 2021. ([video](https://watch.ocaml.org/videos/watch/74ece0a8-380f-4e2a-bef5-c6bb9092be89))
* [Parafuzz: Coverage-guided Property Fuzzing for Multicore OCaml programs](https://icfp21.sigplan.org/details/ocaml-2021-papers/9/Parafuzz-Coverage-guided-Property-Fuzzing-for-Multicore-OCaml-programs), KC Sivaramakrishnan, August 2021. ([video](https://watch.ocaml.org/videos/watch/c0d591e0-91c9-4eaa-a4d7-c4f514de0a57))
* [Adapting the OCaml Ecosystem for Multicore OCaml](https://icfp21.sigplan.org/details/ocaml-2021-papers/7/Adapting-the-OCaml-ecosystem-for-Multicore-OCaml), Sudha Parimala, August 2021. ([video](https://watch.ocaml.org/videos/watch/629b89a8-bbd5-490d-98b0-d0c740912b02))
* [Effective Programming in OCaml](https://kcsrk.info/slides/jet_brains_21.pdf), KC Sivaramakrishnan, April 2021. ([video](https://www.youtube.com/watch?v=plFFZcqBOyk))
* [Multicore OCaml -- what's coming in 2021](https://speakerdeck.com/kayceesrk/multicore-ocaml-whats-coming-in-2021), KC Sivaramakrishnan, December 2020. ([video](https://www.youtube.com/watch?v=mel76DFerL0))
* [Parallelising your OCaml code with Multicore OCaml](https://github.com/ocaml-multicore/multicore-talks/blob/master/ocaml2020-workshop-parallel/slides-with-speaker-notes.pdf), Sadiq Jaffar August 2020. ([video](https://www.youtube.com/watch?v=Z7YZR1q8wzI))
* [Retrofitting parallelism onto OCaml](https://speakerdeck.com/kayceesrk/retrofitting-parallelism-onto-ocaml), KC Sivaramakrishnan, August 2020. ([video](https://www.youtube.com/watch?v=9ClMPz7QaIs))
* [A deep dive into Multicore OCaml GC](https://speakerdeck.com/kayceesrk/multicore-ocaml-gc), KC Sivaramakrishnan, June 2017.
* [Reagents: Lock-free programming for the masses](https://speakerdeck.com/kayceesrk/reagents-lock-free-programming-for-the-masses), KC Sivaramakrishnan, August 2016. ([video](https://www.youtube.com/watch?v=qRWTws_YPBA)).
* [Arrows and Reagents](https://speakerdeck.com/kayceesrk/arrows-and-reagents), KC Sivaramakrishnan, March 2016.
* [Concurrent & Multicore OCaml: A deep dive](https://speakerdeck.com/kayceesrk/concurrent-and-multicore-ocaml-a-deep-dive), KC Sivaramakrishnan, January 2016.
* [Effective Concurrency with Algebraic Effects](http://kcsrk.info/slides/OCaml15.pdf), KC Sivaramakrishnan, OCaml Workshop, September 2015.
* [Multicore OCaml](https://www.cl.cam.ac.uk/~sd601/papers/multicore_slides.pdf), Stephen Dolan, OCaml workshop, September 2014. ([video](https://www.youtube.com/watch?v=FzmQTC_X5R4&list=UUP9g4dLR7xt6KzCYntNqYcw)).


### Discuss Threads

Recently, there has been a lot of great discussion on discuss.ocaml.org around OCaml 5, in particular what the ramifications of adding effects to the language will have to the ecosystem:

 - The [initial release](https://discuss.ocaml.org/t/eio-0-1-effects-based-direct-style-io-for-ocaml-5/9298) of Eio has a lot of dicussion around what it could mean if the library became the official "OCaml IO" library. It also has some useful conversations about the underlying use of effects in Eio and in particular the [set of private effects used](https://v3.ocaml.org/p/eio/0.1/doc/Eio/Private/Effects/index.html).
   + An excellent write up about [scopes and effect handlers](https://hackmd.io/@yF_ntUhmRvKUt15g7m1uGw/Bk-5NXh15) was added in this [thread](https://discuss.ocaml.org/t/eio-0-1-effects-based-direct-style-io-for-ocaml-5/9298/91).
 - Another thread, ["How to block in an agnostic way"](https://discuss.ocaml.org/t/how-to-block-in-an-agnostic-way/9368), is a discussion about blocking code where we are agnostic to the underlying concurrency abstraction (and indeed how to do [interop between libraries](https://discuss.ocaml.org/t/how-to-block-in-an-agnostic-way/9368/4)).
 - Another more Eio-specific thread looks at [cancellation in the presence of concurrency from effects](https://discuss.ocaml.org/t/understanding-cancellation-in-eio/9369).
 - In [How do spawn and join interact with try_with](https://discuss.ocaml.org/t/multicore-how-do-spawn-and-join-interact-with-try-with/9362) there's a great, short discussion about effects in the presence of multiple domains. A "gotcha" many (myself included) are likely to hit when getting started with OCaml 5.
 - There is a great discussion about using effects for a [roguelike](https://en.wikipedia.org/wiki/Roguelike) game in [Tutorial: Roguelike with Effect Handlers](https://discuss.ocaml.org/t/tutorial-roguelike-with-effect-handlers/9422) (the full tutorial [is here](https://hackmd.io/@yF_ntUhmRvKUt15g7m1uGw/BJBZ7TMeq)). Related to that is another excellent article about coding [animations with effect handlers](https://gopiandcode.uk/logs/log-bye-bye-monads-algebraic-effects.html).

### Monthlies

The OCaml monthlies are available on discuss: https://discuss.ocaml.org/tag/multicore-monthly

### Benchmarks

#### Sandmark

Repository: https://github.com/ocaml-bench/sandmark

> Sandmark is a suite of OCaml benchmarks and a collection of tools to configure different compiler variants, run and visualise the results.

Sandmark nightly runs the Sandmark benchmarks and visualises results: https://sandmark.tarides.com/?app=Home

#### Http Benchmarks

Repository: https://github.com/ocaml-multicore/retro-httpaf-bench

A collection of HTTP servers including ones in Go, Rust, multiple OCaml implementations and OCaml with effects.
