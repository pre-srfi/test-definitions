# SRFI nnn: Compatible test definitions

by ...

# Status

Early Draft

# Abstract

This SRFI specifies a few extensions to the venerable SRFI 64 (A
Scheme API for test suites). Some of the extensions are taken from
Alex Shinn's popular test module for CHICKEN and Chibi-Scheme, itself
largely based on SRFI 64.

# Issues

# Rationale

# Specification

### Tests clarified from SRFI 64

These tests are intended to be identical to SRFI 64, but the
specification explicitly addresses edge cases.

Syntax (**test-assert** [_test-name_] _expr_)

Test that _expr_ returns at least one value, and that the primary
value is not `#f`.

Syntax (**test-eq** [_test-name_] _expected-value_ _expr_) +
Syntax (**test-eqv** [_test-name_] _expected-value_ _expr_) +
Syntax (**test-equal** [_test-name_] _expected-value_ _expr_)

Test that _expr_ returns exactly one value, and that value is **eq?**
or **eqv?** or **equal?** to _expected-value_.

### Tests specialized from SRFI 64

Syntax (**test-error** [_test-name_] _expected-error?_ _expr_)

Test that _expr_ raises an exception and the error object satisfies
the _expected-error?_ predicate.

SRFI 64 specified an _error-type_ argument which was optional and had
complex implementation-dependent meaning. This SRFI replaces it with a
required argument which is only allowed to be a predicate. The SRFI 64
specification is lenient enough that this SRFI's version is a
compatible subset of the behavior accepted of SRFI 64 implementations.

If possible, the Scheme implementation should disable compiler
warnings about incorrect arity, unreachable code and other such things
in _expr_ since _expr_ is likely to be intentionally incorrect code.

### New tests

Syntax (**test-tail-error** [_test-name_] _expected-error?_ _expr_)

Like **test-error** but also checks that the exception was raised from
tail position. If the underlying Scheme implementation cannot
distinguish between tail and non-tail exceptions, **test-tail-error**
is equivalent to **test-error**.

Syntax (**test-satisfies** [_test-name_] _expected?_ _expr_) +
Syntax (**test-satisfies-not** [_test-name_] _expected?_ _expr_)

Test that (_expected?_ _expr_) returns at least one value and that the
primary value is `#t` or `#f`, respectively.

Syntax (**test-same-under** [_test-name_] _somehow-equal?_ _expected-value_ _expr_) +

Test that (_somehow-equal?_ _expected-value_ _expr_) returns `#t`.
_somehow-equal?_ should be commutative but this is not strictly
required: the arguments _expected-value_ and _expr_ are guaranteed to
be passed in that order.

### New helpers

Procedure (**test-name** _part..._) -> _string_

Return a string which the concatenation of _parts_ delimited by
spaces. A _part_ can be any object; its **display** representation is
used. For example, `(test-name "foo" 1 #t)` returns `"foo:1:#t"`.

Procedure (**arity-error?** _obj_) -> _boolean_

Return `#t` if _obj_ is an error object representing an arity error.
Else return `#f`. An arity error is what happens when a procedure call
is attempted with one or more missing arguments, or one or more
extraneous arguments.

If the underlying Scheme implementation has one or more special
exception types to represent arity errors, this predicate should test
for those types. If not, it is acceptable to match the error message
against known strings.

In Scheme implementations supporting keyword arguments,
**arity-error?** should return `#t` in case a required keyword
argument is missing. It should also return `#t` in case an unknown
keyword argument was given and the procedure does not accept unknown
keywords.

# Implementation

# Acknowledgements

# References

# Copyright

Copyright (C) Firstname Lastname (2020).

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
