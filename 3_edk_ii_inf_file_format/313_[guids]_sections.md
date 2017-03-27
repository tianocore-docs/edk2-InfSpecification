<!--- @file
  3.13 [Guids] Sections

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

## 3.13 [Guids] Sections

These are optional sections. If the source module code contains any GUIDs
(other than PPI or PROTOCOL GUIDs), then this section must be generated in the
"As Built" INF file listing each GUID with its `<CommentBlock>` content.

#### Summary

Defines the EDK II INF file `[Guids]` section content. This is a list of the
global GUID C Names that are referenced in the module's code, but not already
referenced in the INF. Unique GUID C names may be published in the DEC file (or
come from a Distribution Package surface area description.) The C Names used in
this section are formatted using the external name.

The "common" architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

Each GUID must be listed only once per section. GUIDs listed in architectural
sections are not permitted to be listed in the common architectural section.

The format of the `<CommentBlock>` is the recommended format that will
guarantee that the information is correctly inserted into UEFI Distribution
Package description files by the Intel(R) UEFI Packaging Tool included in the
EDK II base tools project. The usages in the comment block describe how the
GUID is used in the C code.

A "binary" INF file must not contain any `FeatureFlagExpression` content.

```c
<Guids>              ::= "[Guids" [<com_attribs>] "]" <EOL> <GuidsStatements>*
<com_attribs>        ::= {".common"} {<attribs>}
<attribs>            ::= <attrs> [ "," <TS> "Guids" <attrs>]*
<attrs>              ::= "." <arch>
<GuidsStatements>    ::= [<NUsageBlock>}
                         <TS> <GuidSpec> [<1UsageBlock>]
<GuidSpec>           ::= <CName> [<FS> <FeatureFlagExpress>]
                         <1
UsageBlock>          ::= <CommentBlock>
<NUsageBlock>        ::= <CommentBlock>+
<FeatureFlagExpress> ::= <Boolean>
<CommentBlock>       ::= <TS> [<UseOrFieldOpt>] <CmtOrEol>
<UseOrFieldOpt>      ::= [<UsageField>] [<GuidTypeField>]
<UsageField>         ::= "##" <TS> <Usage> <TS>
<GuidTypeField>      ::= "##" <TS> <GuidType> <TS>
<CmtOrEol>           ::= {<Comment>} {<EOL>}
<Usage>              ::= {"CONSUMES"} {"SOMETIMES_CONSUMES"}
                         {"PRODUCES"} {"SOMETIMES_PRODUCES"}
                         {"UNDEFINED"}
<GuidType>           ::= {"Event"} {"File"} {"FV"} {"GUID"} {"HII"}
                         {"HOB"} {"SystemTable"} {"TokenSpaceGuid"}
                         {<VariableType>} {"UNDEFINED"}
<VariableType>       ::= {"Variable"}
                         {"Variable:" <TS> <VariableName>}
<VariableName>       ::= <UnicodeString>
```

#### Parameters

**_FeatureFlagExpress_**

When present, the feature flag expression determines whether the entry line is
valid. If the feature flag expression evaluates to FALSE, this entry will be
ignored by the EDK II build tools.

**_1UsageBlock_** and **_NUsageBlock_**

The `1UsageBlock` location, after the entry, is preferred if there is only one
If a GUID has multiple usages, then all `CommentBlock` statements must precede
the entry.

**_Usage_**

One or more `Usage` comment lines may be specified for a given GUID. If more
than one `Usage`, then the comment section following the `Usage` must be
provided, explaining the different usages.

**_UNDEFINED - Usage_**

Typically, this entry will be used when tools creating/installing UEFI
Distribution Packages encounter a missing or misspelled usage. UNDEFINED may
also be used for GUIDs that identify different types of information.

**_CONSUMES_**

`CONSUMES` means that a module may use this GUID that does not fit into the
defined PROTOCOL or PPI types. The module will use the named GUID. For GUID
type of:

* Event - `CONSUMES` means that the module has an event waiting to be signaled
  (i.e., the module registers a notification function and calls the function
  when it is signaled.

* System Table - this means that the module will use a GUIDed entry in the
  system table.

* Variable - this means that the module may use the variable entry.

* HII - the formset may be registered into HII by this module.

* TokenSpaceGuid - this means that an Token Space GUID will be required for PCD
  entries used in this module.

* Hob - this means that a HOB may need to be present in the system.

* File/FV - this means that a file must be present in an FV, such as a module
  that loads a processor microcode patch file.

* GUID - this means that a module may use this GUID that does not fit into the
  defined GUID types.

**_PRODUCES_**

`PRODUCES` means that a module will produce a GUID that does not fit into the
defined PROTOCOL or PPI types. This module always produces a named GUID. For
GUID type of:

* Event - this means that module will signal all events in an event group.

* System Table - this means that the module will produce a GUIDed entry in the
  system table.

* Variable - this means that the module will write the variable.

* HII - `PRODUCES` is not valid for this GUID type.

* TokenSpaceGuid - `PRODUCES` is not valid for this GUID type.

* Hob - this means that the HOB will be produced by the module.

* File/FV - this means that a module creates a file that is present in an FV,
  such as a file that contains a microcode patch.

* GUID - this means that a module will produce a GUID that does not fit into
  the defined PROTOCOL, PPI or GUID types.

#### Example

```ini
[Guids]
  gEfiDebugImageInfoTable
  gEfiHobMemoryAllocModuleGuid

  gEfiAbcVariableGuid           ## PRODUCES ## Variable:L"XYZ" # Sets the variable

  # Event registered to EFI_HII_SET_KEYBOARD_LAYOUT_EVENT_GUID group,
  # which will be triggered by EFI_HII_DATABASE_PROTOCOL.SetKeyboardLayout().
  ## SOMETIME_CONSUMES ## Event
  gEfiHiiKeyBoardLayoutGuid
```
