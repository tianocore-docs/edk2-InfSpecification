<!--- @file
  3.7 [Packages] Sections

  Copyright (c) 2007-2017, Intel Corporation. All rights reserved.<BR>

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

## 3.7 [Packages] Sections

These are optional sections. If there are files listed under a `[Sources]`
section, then the INF file is required to list the MdePkg/MdePkg.dec file as
the first file in a `[Packages]` section. This section is also required if the
module uses PCDs for both source and the binary "As Built" INF modules.

#### Summary

Defines the `[Packages]` section tag that is used in EDK II module INF files.
Each entry in this section contains a directory name, forward slash character
and the name of the DEC file contained in the directory name.

Packages must be listed in the order that may be required for specifying
include path statements for a compiler. For example, the _MdePkg/MdePkg.dec_
file must be listed before the `MdeModulePkg/MdeModulePkg.dec` file. If there
are PCDs listed in the generated "As Built" INF, the packages that declare any
PCDs must be listed in this section.

Each package filename must be listed only once per section. Package filenames
listed in architectural sections are not permitted to be listed in the common
architectural section.

The `"common"` architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

Packages listed under the `"common"` architecture section must not be listed in
sections that have other architecture modifiers.

#### Prototype

```c
<Packages>           ::= "[Packages" [<com_attrs>] "]" <EOL> <Statements>*
<com_attrs>          ::= {".common"} {<attrs>}
<attrs>              ::= <Archs> ["," <TS> "Packages" <Archs>]
<Archs>              ::= "." <arch>
<Statements>         ::= {<MacroDefinition>} {<PkgStatements>}
<PkgStatements>      ::= <TS> <Filename> [<Field2>] <EOL>
<Field2>             ::= <FS> <FeatureFlagExpress>
<FeatureFlagExpress> ::= <Boolean>
```

#### Parameters

**_Filename_**

Paths listed in the `[Packages]` section contain a directory name, forward
slash character and the name of the DEC file contained in the directory name.
Use of "..", "." and "../" in the directory path is not permitted. Use of an
absolute path is prohibited.

**_FeatureFlagExpress_**

When present, the feature flag expression determines whether the entry line is
valid. If the feature flag expression evaluates to FALSE, this entry will be
ignored by the EDK II build tools.

#### Example

```ini
[Packages]
  MdePkg/MdePkg.dec
  MdeModulePkg/MdeModulePkg.dec

[Packages.IA32]
  DEFINE CPUS = IA32FamilyCpuPkg
  $(CPUS)/DualCore/DualCore.dec
```
