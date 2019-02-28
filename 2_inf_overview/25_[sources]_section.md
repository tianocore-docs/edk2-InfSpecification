<!--- @file
  2.5 [Sources] Section

  Copyright (c) 2007-2019, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

## 2.5 [Sources] Section

The `[Sources]` section is used to specify the files that make up the
component. Directories names are required for files existing in subdirectories
of the component. All directory names are relative to the location of the INF
file. Each file is added to the macro of `$(INC_DEPS)`, which can be used in a
makefile dependency expression.

Binary files must not be listed in this section. EDK II INF files may have a
`[Binaries]` section defined that must be used to define the type and name of
the binary files provided by a module.

This section is optional. If it is present, and files are listed in this
section, then the build tools must process the files for AutoGen as well as
`Makefile` generation. If this section is not present, then the build tools may
assume that the binary files listed in the `[Binaries]` section have already
been processed by the first build step - no AutoGen or Makefiles need to be
generated.

If both `[Sources]` and `[Binaries]` sections are specified, the build tools
assume that the code provided in the sources section must be built, and that
the binary files provided are also required for the final image generation
process steps.

Files listed in architectural specific sections must not be listed in common
architecture `[Sources]` sections. The architectural modifier is used to
specify additional files that are required over and above the non-architectural
specific content. During builds, files are grouped by tools using the common
and architecturally specified sections.

This section will typically use one of the following section definitions:

```ini
[Sources]
[Sources.common]
[Sources.IA32]
[Sources.X64]
[Sources.EBC]
```

The formats for entries in this section are:

```ini
Relative/path/and/filename.ext
Filename.ext
```

The following is an example for sources sections.

```ini
[Sources.common]
  DxeIpl.dxs
  DxeIpl.h
  DxeLoad.c

[Sources.Ia32]
  Ia32/VirtualMemory.h
  Ia32/VirtualMemory.c
  Ia32/DxeLoadFunc.c Ia32/ImageRead.c

[Sources.X64]
  X64/DxeLoadFunc.c

```

All Unicode files must be listed in the source section. If a Unicode file,
`A.uni`, has the statement: `#include B.uni`, and `B.uni` has a statement:
`#include C.uni`, both `B.uni` and `C.uni` files must be listed in the INF
`[Sources]` section in addition to the `A.uni` file.

Specifying a file in an architectural section and in the common architecture
section is prohibited (a file cannot be specific to a single architecture and
also be general for all architectures).
