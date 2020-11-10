# WASI SDK

[![Build Status](https://dev.azure.com/CraneStation/wasi-sdk/_apis/build/status/CraneStation.wasi-sdk?branchName=master)](https://dev.azure.com/CraneStation/wasi-sdk/_build/latest?definitionId=2&branchName=master)

## Quick Start

[Download SDK packages here.](https://github.com/WebAssembly/wasi-sdk/releases)

## About this repository

This repository contains no compiler or library code itself; it uses
git submodules to pull in the upstream Clang and LLVM tree, as well as the
wasi-libc tree.

The libc portion of this SDK is the
[wasi-libc](https://github.com/WebAssembly/wasi-libc).

Upstream Clang and LLVM (from 9.0 onwards) can compile for WASI out of the box,
and WebAssembly support is included in them by default. So, all that's done here
is to provide builds configured to set the default target and sysroot for
convenience.

One could also use a standard Clang installation, build a sysroot from the
sources mentioned above, and compile with
"--target=wasm32-wasi --sysroot=/path/to/sysroot".

## Install

A typical installation from the release binaries might look like the following:
```shell script
wget https://github.com/WebAssembly/wasi-sdk/releases/download/wasi-sdk-[VERSION]/wasi-sdk-[VERSION]-linux.tar.gz
tar xvf wasi-sdk-[VERSION]-linux.tar.gz
```

## Use

Use the clang installed in the wasi-sdk directory:
```shell script
CC="[WASI_SDK_PATH]/bin/clang --sysroot=[WASI_SDK_PATH]/share/wasi-sysroot"
$CC foo.c -o foo.wasm
```
Note: `[WASI_SDK_PATH]/share/wasi-sysroot` contains the WASI-specific includes/libraries/etc. The `--sysroot=...` option
is not necessary if `WASI_SDK_PATH` is `/opt/wasi-sdk`.

## Notes for Autoconf

Upstream autoconf now
[recognizes WASI](http://lists.gnu.org/archive/html/config-patches/2019-04/msg00001.html).

For convenience when building packages that aren't yet updated, updated
config.sub and config.guess files are installed at share/misc/config.\*
in the install directory.

## Notable Limitations

This repository does not yet support C++ exceptions. C++ code is
supported only with -fno-exceptions for now. Similarly, there is not
yet support for setjmp/longjmp. Work on support for [exception handling] 
s underway at the language level which will support both of these
features.

[exception handling]: https://github.com/WebAssembly/exception-handling/

This repository does not yet support [threads]. Specifically, WASI does
not yet have an API for creating and managing threads yet, and WASI libc
does not yet have pthread support.

[threads]: https://github.com/WebAssembly/threads

This repository does not yet support dynamic libraries. While there are
[some efforts](https://github.com/WebAssembly/tool-conventions/blob/master/DynamicLinking.md)
to design a system for dynamic libraries in wasm, it is still in development
and not yet generally usable.

There is no support for networking. It is a goal of WASI to support networking
in the future though.
