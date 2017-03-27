<!--- @file
  3.10 [UserExtensions] Sections

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

## 3.10 [UserExtensions] Sections

These are optional sections.

#### Summary

Defines the optional EDK II INF file `[UserExtensions]` section tag. The build
tools must have an a priori knowledge of how to process any items in this
section.

Each UserExtensions section must have a unique set of `UserId`, `IdString` and
Arch values.

The "common" architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

This means that the same `UserId` can be used in more than one section,
provided the `IdString` or Arch values are different. The same `IdString` values
can be used if the `UserId` or Arch values are different. The same `UserId` and
the same `IdString` can be used if the Arch values are different.

Any `[UserExtensions]` sections that are present in the source INF with a
UserId of "TianoCore" will be copied into the "As Built" INF file.
`[UserExtensions]` sections with other UserId values will not be copied to the
"As Built" INF file.

Files listed in a `[UserExtensions.TianoCore."ExtraFiles"]` section must be
included in a UEFI Distribution Package.

```c
<UserExtensions> ::= "[UserExtensions" <com_attribs> "]" <EOL>
                     <statements>*
<com_attribs>    ::= {<com_arch>} {<attribs>}
<com_arch>       ::= <IdContent> [".common"]
<attribs>        ::= <IdContent> ["," <TS> "UserExtensions"
                     <IdContent>]*
<IdContent>      ::= <UserId> <IdString> [<attrs>]
<attrs>          ::= "." <arch>
<UserId>         ::= "." {(a-zA-Z)(a-zA-Z0-9_.)*} {"TianoCore"}
<IdString>       ::= "." {<NormalizedString>} {<SimpleWord>}
                     {<ReservedWord>}
<ReservedWord>   ::= {"PRE_PROCESS"} {"POST_PROCESS"}
                     {"ExtraFiles"}
<statements>     ::= Content is build tool chain specific.
```

#### Parameters

**_UserId_**

Words that contain period "." must be encapsulated in double quotation marks.

**_IdString_**

Normalized strings that contain period "." or space characters must be
encapsulated in double quotation marks. The `IdString` must start with a letter.

#### Example

```ini
[UserExtensions.Edk2AcpiTable."1.0"]
   Any content may go here
```

### 3.10.1 [UserExtensions.TianoCore."ExtraFiles"] Section

This is an optional section.

Defines the optional EDK II INF file `[UserExtensions.TianoCore."ExtraFiles"]`
section tag. The EDK II build tools must not process any files listed in this
section.

#### Summary

This section is used by the Intel(R) UEFI Packaging Tool, that is distributed
as part of the EDK II BaseTools, to locate files listed under this section
header and add them to the UEFI distribution package. When installing a UEFI
distribution package, these files will be installed in the module's directory
tree.

#### Prototype

```c
<UserExtensions> ::= "[UserExtensions" <TcEf> "]" <EOL> <FileNames>*
<TcEf>           ::= ".TianoCore." <DblQuote> "ExtraFiles" <DblQuote>
<FileNames>      ::= <TS> [<RelativePath>] <File> <EOL>
```

#### Parameters

**_FileNames_**

Paths listed in the filename elements of the this section must be relative to
the directory the INF file resides in. Use of "..", "." and "../" in the
directory path is not permitted.

#### Example

```ini
[UserExtensions.TianoCore."ExtraFiles"]
  Readme.txt
```
