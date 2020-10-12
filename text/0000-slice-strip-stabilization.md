- Feature Name: `slice-strip-stabilization`
- Start Date: 2020-10-12
- RFC PR: [rust-lang/rfcs#0000](https://github.com/rust-lang/rfcs/pull/0000)
- Rust Issue: [rust-lang/rust#73413](https://github.com/rust-lang/rust/issues/73413)
	
# Summary
[summary]: #summary

Stabilize the `strip_prefix` and `strip_suffix` methods on `&[]` slices.

# Motivation
[motivation]: #motivation

The `strip_*` mthods are useful.  The corresponding methods on `str` are already stable.

# Guide-level Explanation
[guide-level-explanation]: #guide-level-explanation

This is a stabilisation RFC.  The following methods will no longer be gated behind
`#![feature(slice_strip)]` and will be available on stable Rust:

 * [`slice::strip_prefix`](https://doc.rust-lang.org/std/primitive.slice.html#method.strip_prefix)
 * [`slice::strip_suffix`](https://doc.rust-lang.org/std/primitive.slice.html#method.strip_suffix)

Note that the documentation is, separately, hopefully going to be improved slightly.  See [rust#75078](https://github.com/rust-lang/rust/pull/75078).

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

Drop the `unstable` and `cfg` annotations for the unstable feature `slice_strip`, and stabilise these functions with the current API.

# Drawbacks
[drawbacks]: #drawbacks

## Risk of getting in the way of rich pattern api for `&[]` slices?

The `strip_*` methods on `str` participate in a rich pattern API.  This pattern API is not yet
available for `&[]` slices.  Perhaps stabilising these methods on slices now would mean that
providing a pattern API for slices, later, would be a breaking change.  If that is the case then we
should do the slice pattern API first.

However, I don't think this is actually the case: as I understand it, We can stabilise `slice_strip`
taking only `&[T]` as the pattern.  And then we can, later, introduce a suitable trait, implemented
by `&[T]`, and that would not be a breaking API change.

There are already stable functions on `&[]` slices which take specific kinds of pattern: `split`
etc. take a predicate function.

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

I even accidentally used `strip_prefix` on a slice because I was reading the docs for `str`.  I was
then disappointed to see the message from the compiler telling me it was unstable.

The only sensible alternative to stabilisation now is to wait for a rich slice pattern API.

# Prior art
[prior-art]: #prior-art

Rust has already decided on the names and argument order etc. for these functions, on `str`.

# Unresolved questions
[unresolved-questions]: #unresolved-questions

We need confirmation that stabilising this now would not get in the way of a pattern API later.

# Future possibilities
[future-possibilities]: #future-possibilities

It would be nice for slices to support a rich pattern API rather like the one for string slices.
