<!--- @file
  2.15 [Depex] Section

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

## 2.15 [Depex] Section

This section is used for specifying a `Depex` expression, not a binary file. In
the "As Built" INF files, this section contains a comment that lists the full
dependency expression, including `Depex` statements AND'd from library
instances linked against a module.

Binary .depex files are listed in `[Binaries]` sections of the INF files.

Having a common `[Depex]` section and architectural `[Depex]` sections is
prohibited. Having multiple module type modifiers for common and architectural
sections is permitted. For example,
`[Depex.common.DXE_DRIVER, Depex.common.DXE_RUNTIME_DRIVER]` is valid.

This section can be used with an inheritance from libraries, by supporting
logical AND'ing of the different Depex expressions together. Since more than
one type of dependency expression may be required for modules DXE/SMM modules,
as well as components of type `COMBINED_PEIM_DRIVER` (not supported by the EDK
II build system), section modifier tags have been defined. For module types
that prohibit the use of a `[Depex]` section, all `[Depex]` sections from
library instances must be ignored. These are only required if more than one
dependency expression is required for a module.

The format of the depex section tag is:

`Depex[.<Arch>[.<ModuleType>]]`

Additionally, the rules for specifying DEPEX sections are as follows.

* If the Module is a Library, then a `[Depex]` section is optional.

* If the Module is a Library with a MODULE_TYPE of BASE, the generic (i.e.,
  [Depex]) and generic with only architectural modifier entries (i.e.,
  [Depex.IA32]) are not permitted. It is permitted to have a Depex section if
  one ModuleType modifier is specified (i.e., [Depex.common.PEIM).

* If the ModuleType is `USER_DEFINED`, then a `[Depex]` section is optional. If
  a PEI, SMM or DXE DEPEX section is required, the user must specify a
  ModuleType of `PEIM` to generate a `PEI_DEPEX` section, a ModuleType of
  `DXE_DRIVER` to generate a `DXE_DEPEX` section, or a ModuleType of
  `DXE_SMM_DRIVER` to generate an `SMM_DEPEX` section.

* If the ModuleType is `SEC`, `UEFI_APPLICATION`, `UEFI_DRIVER`, `PEI_CORE`,
  `SMM_CORE`, `DXE_CORE`, `HOST_APPLICATION`, no `[Depex]` sections are
  permitted and all library class `[Depex]` sections are ignored.

* Module types `PEIM`, `DXE_DRIVER`, `DXE_RUNTIME_DRIVER`, `DXE_SAL_DRIVER` and
  `DXE_SMM_DRIVER` require a `[Depex]` section unless the dependencies are
  specified by a `PEI_DEPEX`, `DXE_DEPEX` or `SMM_DEPEX` in the `[Binaries]`
  section.

The Depex section headers start with one of the following:

```ini
[Depex]
[Depex.IA32]
[Depex.X64]
[Depex.EBC]
[Depex.common]
```

When generating the "As Built" binary INF during a build, the complete
dependency expression, including dependencies from library instances, will be
listed in comments.

The following are examples of Depex section:

```ini
[Depex]
  TRUE

[Depex.IA32.DXE_DRIVER, Depex.IA32.DXE_RUNTIME_DRIVER]
  gEfiPcdProtocolGuid
```
