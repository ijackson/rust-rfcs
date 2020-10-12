- Feature Name: (fill me in with a unique ident, `my_awesome_feature`)
- Start Date: (fill me in with today's date, YYYY-MM-DD)
- RFC PR: [rust-lang/rfcs#0000](https://github.com/rust-lang/rfcs/pull/0000)
- Rust Issue: [rust-lang/rust#0000](https://github.com/rust-lang/rust/issues/0000)

# Summary
[summary]: #summary

Stabilize the `split_inclusive` methods on `str` and `&[]` slices.

# Motivation
[motivation]: #motivation

The `split_inclusive_*` methods are convenient.

# Guide-level Explanation
[guide-level-explanation]: #guide-level-explanation

This is a stabilisation RFC.  The following methods will no longer be gated behind
`#![feature(slice_strip)]` and will be available on stable Rust:

 * `slice::split_inclusive`
 * `slice::split_inclusive_mut`
 * `str::split_inclusive`

Tracking issue:https://github.com/rust-lang/rust/issues/72360

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

Drop the unstable annotations, and config settings for the unstable feature `split_inclusive`.

# Drawbacks
[drawbacks]: #drawbacks

## `split_*` method proliferation and lack of generality

The `split*` family of functions is starting to become a bit unwieldy, with a matrix of possible
choices: backwards/forwards (split vs rsplit), limited/unlimited (split vs splitn), mutable or not.
If we add `split_inclusive`, what about `rsplit_inclusive`, `splitn_inclusive`, etc. ?  These seem
logically necessary.

## Missing `lines_inclusive`

It would be nice to have `lines_inclusive` on `str`, but that is not implemented yet.  Perhaps we
should defer stabilisation until that is implemented.

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

## Perhaps we need a generalised version

```
   pub fn split_general('a,P>(&'a self, pat: P, direction: Direction, limit: Option<usize>, inclusive: Inclusiveness)
       where P: Pattern
```
or even soemthing with generics
```
   pub fn split_generic('a,P,H>(&'a self, pat: P, how: H) -> ...
       where P: Pattern
       where H: SplitHow;

   pub fn split_generic_mut('a,P,H>(&'a mut self, pat: P, how: H) -> ...
       where P: Pattern
       where H: SplitHow;

   trait SplitHow {
       fn reverse(&self) -> bool;
       fn limit(&self) -> Option<usize>;
       fn inclsuive -> bool;
   }

   pub struct Split;
   ...
   pub struct SplitRevInclN(pub usize);
```

But this seems like a can of worms, and one which we can defer opening if we don't mind having a lot
of methods on `str` and `&[]`.

## Perhaps we should avoid expanding the standard library

I don't think this is an attractive proposition.

# Prior art
[prior-art]: #prior-art

Golang calls `split_inclusive` function `SplitAfter`.  It provides `SplitAfterN` too.  Golang does
not provide reverse splitting.  `split_inclusive` seems like a better name to me.

Languages where the split operator takes a regular expression can often use look-behind assertions
to achieve the desired effect, but it's not very readable or ergonomic (e.g. Javascript, Python).

# Unresolved questions
[unresolved-questions]: #unresolved-questions

[ ] Are we happy stabilising `split_inclusive` without `rsplitn_inclusive` et al ?
[ ] What should it be called ?

# Future possibilities
[future-possibilities]: #future-possibilities

Stabilising this function now does not preclude completing the family of functions:
 * `rsplit_inclusive` (and `_mut` on `&[]`)
 * `splitn_inclusive` (and `_mut` on `&[]`)
 * `rsplitn_inclusive` (and `_mut` on `&[]`)

On the other hand, it does not imply adding those either: stabilising `split_inclusive[_mut]` now
does not preclude providing a more general arrangment, in which case `split_inclusive[_mut]` becomes
a convenience alias for a more comprehensive function.
