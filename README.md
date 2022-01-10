# Awesome Multicore OCaml

A collection of libraries, experiments and ideas relating to OCaml 5 (multicore + effects). With the [merging](https://github.com/ocaml/ocaml/pull/10831#issuecomment-1008935795) of the main `multicore-ocaml` branch into OCaml `trunk`, a parallel and effectful world is fast approaching.

This repository collects useful libraries, ideas and experiments relating to the use of Multicore OCaml features: primarily parallelism with domains and direct-style code with effects.

## Table of Contents
- [Libraries](#libraries)
  - [Eio](#eio)
  - [Domainslib](#domainslib)
  - [Lwt Support](#lwt-support)
- [Experiments](#experiments)
  - [Ppx_effects](#ppx_effects)
  - [Gemini Protocol](#gemini-protocol)
  - [Multi-shot Continuations](#multi-shot-continuations)
- [Resources](#resources)
    - [Wiki](#wiki)
    - [Monthlies](#monthlies)
    - [Benchmarks](#benchmarks)

## Libraries

### Eio

Repository: https://github.com/ocaml-multicore/eio

Eio implements an effects-based direct-style IO stack for multicore OCaml. It supports multiple backends including a performant [io-uring](https://unixism.net/loti/what_is_io_uring.html) one and a portable, [libuv](http://docs.libuv.org/en/v1.x/) backend.

### Domainslib

Repository: https://github.com/ocaml-multicore/domainslib

Domainslib provides data-structures for parallel programming on top of multicore primitives. This includes a work-stealing task pool and channels for inter-domain communication.

### Lwt Support

#### Lwt_eio

Repository: https://github.com/talex5/lwt_eio

`Lwt_eio` provides an "lwt engine" using [Eio](#eio) making it possible to use both Eio and Lwt in the same codebase (there are quite a few caveats however).

#### Lwt_domain

Repository: https://github.com/ocsigen/lwt

Not yet released, [upstream lwt](https://github.com/ocsigen/lwt/tree/master/src/domain) now has a package called `lwt-domain`. This provides useful functions for programming with Lwt and [Domainslib](#domainslib).

## Experiments

### Ppx_effects

Repository: https://github.com/CraigFe/ppx_effects

The effects in OCaml 5 will not support a dedicated syntax, but plans exist for this perhaps once the effects are typed. Until then this ppx provides an approximation to the proposed syntax.

### Gemini Protocol

Repository: https://gitlab.com/talex5/gemini-eio

The [gemini protocol](https://gemini.circumlunar.space/docs/faq.gmi) implemented in direct-style using [eio](#eio).

### Multi-shot Continuations

Repository: https://github.com/dhil/ocaml-multicont

Built on top of the continuations in Multicore OCaml, `ocaml-multicont` provides a library for using continuations that can be applied more than once. See also [this discussion on discuss](https://discuss.ocaml.org/t/multi-shot-continuations-gone-forever/9072).

## Resources

### Wiki

The [Multicore OCaml Wiki](https://github.com/ocaml-multicore/ocaml-multicore/wiki) contains a lot of useful information from design decisions to examples. Two notable repositories are: 

  - [Effects examples](https://github.com/ocaml-multicore/effects-examples): a collection of great examples with effects including cooperative threading and generators.
  - [Parallel Programming in Multicore OCaml](https://github.com/ocaml-multicore/parallel-programming-in-multicore-ocaml): a very good introduction to programming parallel programs in Multicore OCaml, including [using Domainslib](https://github.com/ocaml-multicore/parallel-programming-in-multicore-ocaml#domainslib).

### Monthlies

The OCaml monthlies are available on discuss: https://discuss.ocaml.org/tag/multicore-monthly

### Benchmarks

Repository: https://github.com/ocaml-bench/sandmark

> Sandmark is a suite of OCaml benchmarks and a collection of tools to configure different compiler variants, run and visualise the results.