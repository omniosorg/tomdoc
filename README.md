# Man page conversion helper

This repository holds a small stand-alone utility which helps with converting
illumos man pages from the legacy [man(7)](https://man.omnios.org/man.7) format
to the newer [mdoc(7)](https://man.omnios.org/mdoc.7).

It does not aim to completely automate conversion, the output will need
reviewing and more work, but it does take care of the more laborious conversion
tasks, speeding up the process.

This tool will likely continue to evolve (and pull requests or feature requests
are very welcome) but it's already pretty good at reformatting long paragraphs
or textual sections (such as `DESCRIPTION`), fixing up formatting and converting
cross references. It also has a fairly good stab at converting lists (although
it currently assumes that they are in a specific form) and it can handle
nested lists and bullet points. Even when it gets this a bit wrong, there is
still a lot less work needed by the human editor afterwards.

It does not currently attempt to transform the `SYNOPSIS` or `ATTRIBUTES`
sections beyond formatting.
The [illumos-gate prototype templates](https://github.com/illumos/illumos-gate/tree/master/usr/src/prototypes)
provide useful guidance on completing conversion of `ATTRIBUTES` in particular.

### Usage

`tomdoc` can be used as a filter or by providing a filename as the only
argument. Usage is generally something like one of these:

```
% ./tomdoc /usr/share/man/man1/zonename.1 > zonename.1

% ./tomdoc /usr/share/man/man1/zonename.1 | less

% ./tomdoc < /usr/share/man/man1/zonename.1 | mandoc | less
```

### Example

Processing the DESCRIPTION section from [adb(1)](https://man.omnios.org/adb).
There is one instance of `.Sy mdb ,` in the output there that I would possibly
manually adjust (one feature of the old format man pages is very liberal use
of emboldening that I have generally not seen carried forward in conversions),
but otherwise it's exactly what I would have done by hand.

```
% ./tomdoc /usr/share/man/man1/adb.1 | sed -n '/Sh DESCRIPTION/,/^\.Sh/p'
.Sh DESCRIPTION
The
.Nm
utility is an interactive, general-purpose debugger.
It can be used to examine files and provides a controlled environment for
the execution of programs.
.Pp
The
.Nm
utility is now implemented as a link to the
.Xr mdb 1
utility.
.Xr mdb 1
is a low-level debugging utility that can be used to examine user processes
as well as the live operating system or operating system crash dumps.
The new
.Xr mdb 1
utility provides complete backwards compatibility with the existing syntax
and features of
.Nm ,
including support for processing
.Nm
macro files.
The
.%T Modular Debugger Guide
and
.Xr mdb 1
man page describe the features of
.Sy mdb ,
including its
.Nm
compatibility mode.
This mode will be activated by default when the
.Nm
link is executed.
```

