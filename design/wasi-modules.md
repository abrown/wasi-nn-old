WASI API Modules
================

A WASI module is set of APIs that can be imported a WebAssembly.  Currently
these API consistent of WebAssembly Functions but in the future this could be
extents to other types of imports such as Globals.

Naming
------

WebAssembly imports are specified by a pair of strings: a *module name* and
*field name*.  All import associated with a given WASI module are available
under a single module name.  To avoid conflicting with other imports this name
is prefixed with `wasi:`.  For example, a WASI module called `foo` would be
available under `wasi:foo`.

Versioning
----------

As WASI modules evolve the aim is to make strong guarantees about compatibility.
Once an API is defined its semantics are cannot be changed.  Any changes to the
module must be made in a backwards compatible way. Backwards compatible changes
include are limited to:

 - Adding new functions to the API.
 - Increasing the range of input value accepted by an existing function.

Programs that depend on newer functions can simply import that function.  These
programs will fail to link on platforms that don't support the newer function.
Programs that need to run on both older and newer implementations may use [weak
imports](weak-imports.md) to achieve this goal.

Incompatible Changes
~~~~~~~~~~~~~~~~~~~~

Any backward incompatible change, such as removing a function or changing a
function signature, requires the module to be renamed.  By convention this done
by add a `/vN` suffix to the module name. For example `wasi:foo/v2`.  The
rational for including the major version in the import name is inspired by the
[*semantic import versioning*](https://blog.golang.org/versioning-proposal)
proposal for the Go language.

Since incompatible versions of the same module are imported under different
names it is legal for a single program to depend on more than one version of a
module.  For example it is leagel to import functions from both `wasi:foo` and
`wasi:foo/v2`.

Current WASI modules
--------------------

- [WASI-core](WASI-core.md)
