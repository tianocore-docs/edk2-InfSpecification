<!--- @file
  3.6 [LibraryClasses] Sections

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

## 3.6 [LibraryClasses] Sections

These are optional sections.

#### Summary

Defines the EDK II `[LibraryClasses]` section content. The Library Class
entries are single lines with one or two fields, separated by the pipe "|"
character.

The EDK II build system will provide an option to generate an "As Built" INF
that can be used to distribution binary modules. Since a binary distribution
does not build, the library instances that were linked into the binary are
listed in comments, rather than as library class keywords and recommended
instances. Tools that create "As Built" information must expand any macro
values used by the tools during the module build. Listing a library class
keyword outside of the "As Built" information is prohibited.

Each library class keyword must only be listed once in a library classes
section. Library class keywords listed in architectural sections are not
permitted to be listed in the common architectural section.

The `"common"` architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

The use of any form of the word `"NULL`" (as in "Null" or "null") as a keyword
in the entries is prohibited.

#### Prototype

```c
<LibraryClasses>     ::= "[LibraryClasses" [<com_attrs>] "]" <EOL>
                         [<Statements>]
<com_attrs>          ::= {".common"} {<attrs>}
<attrs>              ::= <Archs> ["," <TS> "LibraryClasses" <attrs>]*
<Archs>              ::= "." <arch>
<Statements>         ::= {<SourceContent>*} {<AsBuiltInfo>}
<SourceContent>      ::= <TS> {<SourceStmts>} {<MacroDefinition>}
<SourceStmts>        ::= [<RecInstanceCmt> <Filename> <EOL>] <TS> <Keyword>
                         [<Field2>] <EOL>
<RecInstanceCmt>     ::= "##" <MTS> "@RecommendedInstance" <MTS>
<NoN>                ::= (A-MO-Z)
<NoU>                ::= (a-tv-zA-TV-Z0-9)
<NoL>                ::= (a-km-zA-TV-Z0-9)
<Keyword>            ::= {(A-Z) (a-zA-Z0-9){0,2}}
                         {(A-Z) (a-zA-Z0-9){4,}}
                         {<NoN> (a-zA-Z0-9){1,}}
                         {(A-Z) <NoU> (a-zA-Z0-9){0,}}
                         {(A-Z) (a-zA-Z0-9) <NoL> (a-zA-Z0-9){0,}}
                         {(A-Z) (a-zA-Z0-9){2} <NoL>(a-zA-Z0-9){0,}}
<Field2>             ::= <FS> <FeatureFlagExpress>
<FeatureFlagExpress> ::= <Boolean>
<AsBuiltInfo>        ::= <TS> "##" <MTS> "@LIB_INSTANCES" <WS>
                         <LibInstance>*
<LibInstance>        ::= <TS> "#" <TS> <InfFile> <EOL>
```

#### Parameters

**_Field1_**

This is a keyword that uniquely identifies a library class required to
successfully execute the driver.

**_Filename_**

Filenames listed in the Recommended Instance comment in the `[LibraryClasses]`
section must be a package relative path. Use of "..", "." and "../" in the
directory path is not permitted. If the Recommended Instance INF file is a
member a Package (the Package contains a DEC file) the Package (DEC file) of
must also be present in the `[Packages]` section. Use of an absolute path is
prohibited.

**_FeatureFlagExpress_**

When present, the feature flag expression determines whether the entry line is
valid. If the feature flag expression evaluates to FALSE, this entry will be
ignored by the EDK II build tools.

#### Example

```ini
[LibraryClasses.common]
  DEFINE MDE = MdePkg/Library
  ## @RecommendedInstance $(MDE)/BaseDebugLibNull/BaseDebugLibNull.inf
  DebugLib
  UefiDriverModelLib
  PcdComponentNameDisable
  ## @RecommendedInstance $(MDE)/UefiDriverModelLib/UefiDriverModelLib.inf
  PcdDriverDiagnosticsDisable
  UefiDriverEntryPoint
  UefiLib
  ## @RecommendedInstance $(MDE)/BaseLib/BaseLib.inf
  BaseLib
```
