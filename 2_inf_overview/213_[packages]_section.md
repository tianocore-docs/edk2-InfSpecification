<!--- @file
  2.13 [Packages] Section

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

## 2.13 [Packages] Section

The `[Packages]` section lists all of the EDK II declaration files that are
used by the component. Data from the INF and the DEC files is used to generate
content for the `AutoGen.c` and `AutoGen.h` files.

Packages listed in architectural sections must not be listed in common
`[Packages]` sections. The architectural section modifier is used as a
restriction to mask items from architectures that are not applicable. The
locations of the packages listed in this section will be used in generating
include path statements for compiler tool chains. The packages must be listed
in the order that resolves any include dependencies.

This section uses one of the following section definitions:

```ini
[Packages]
[Packages.common]
[Packages.IA32]
[Packages.X64]
[Packages.EBC]
```

The path must include the DEC file name and the name of the directory that
contains the DEC file.

```
MdeModulePkg/MdeModulePkg.dec  # MdeModulePkg
MdePkg/MdePkg.dec              # MdePkg
```

The following is an example of a packages section:

```ini
[Packages]
  MdeModulePkg/MdeModulePkg.dec
  MdePkg/MdePkg.dec
```

If there are files listed under the `[Sources]` section, then the
`MdePkg/MdePkg.dec` file must be specified in this section. The MdePkg contains
information that is required by the EDK II build system in order to compile or
assemble source files using external compilers or assemblers. When generating
the "As Built" binary INF, the tools must include all packages that declare
PCDs used by this module.

Binary only INF files must include this section if a `[PatchPcd]` or `[PcdEx]`
section contains PCD entries.
