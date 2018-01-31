<!--- @file
  3.8 PCD Sections

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

## 3.8 PCD Sections

The PCD sections are optional. If the source module code contains any Patchable
in Module or DynamicEx PCDs, then this section must be generated in the
"As Built" INF file listing each PCD with its `<CommentBlock>` content if
available. Refer to the EDK II Build Specification, section 8.4.1 for PCD
processing rules.

#### Summary

Defines the `[(PcdType)]` section content for EDK II module INF files. If a
default value is specified on the entry line, it must match the datum type
specified in the DEC file that declares this PCD. The Datum Type values are
defined in the PI Specification. PCD expressions are single lines with two or
three fields; fields are separated using the pipe "|" character. Empty Fields
are permitted.

The EDK II Build system will generate an "As Built" INF file that can be
delivered with a binary distribution. Only `[PatchPcd]` and [`PcdEx]` section
types are valid for in "As Built" INF file. Tools that create "As Built"
information must expand any macro values used by the tools during the module
build.

The format of the `<CommentBlock>` is the recommended format that will
guarantee that the information is correctly inserted into UEFI Distribution
Package description files by the Intel(R) UEFI Packaging Tool included in the
EDK II base tools project.

Each PCD name must only be listed once in a section. PCD names listed in
architectural sections must not be listed in the common architectural section.
PCDs should be either architectural in nature or common to all architectures.
The module developer should note that all recommended values may be overridden
by values specified by a platform integrator in a platform description (DSC)
file.

The "common" architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

It is not permissible to list a PCD name in different PCD type sections.
Listing a PCD indicates what library function is used to access a PCD; only one
type of access is permitted for a specified PCD.

A generated "As Built" INF file must not contain any `FeatureFlagExpression`
content.

#### Prototype

```c
<Pcds>                ::= {<AsBuiltPcdSec>} {<SrcPcdSec>}
                          {<FeaturePcd>}
<SrcPcdSec>           ::= {<Patch>} {<Fixed>} {<Dyn>} {<DynEx>}
                          <PcdEntriesStmts>*
<PcdEntriesStmts>     ::= <PcdEntries>
<Patch>               ::= "[PatchPcd" [<com_patchAttrs> "]" <EOL>
<com_patchAttrs>      ::= {".common"} {<patchAttrs>}
<patchAttrs>          ::= <attrs> ["," <TS> "PatchPcd" <attrs>]*
<Fixed>               ::= "[FixedPcd" [<com_fixedAttrs> "]" <EOL>
<com_fixedAttrs>      ::= {".common"} {<fixedAttrs>}
<fixedAttrs>          ::= <attrs> ["," <TS> "FixedPcd" <attrs>]*
<Dyn>                 ::= "[Pcd" [<com_pcdAttrs> "]" <EOL>
<com_pcdAttrs>        ::= {".common"} {<pcdAttrs>}
<pcdAttrs>            ::= <attrs> ["," <TS> "Pcd" <attrs>]*
<DynEx>               ::= "[PcdEx" [<com_pcdexAttrs> "]" <EOL>
<com_pcdexAttrs>      ::= {".common"} {<pcdexAttrs>}
<pcdexAttrs>          ::= <attrs> ["," <TS> "PcdEx" <attrs>]*
<FeaturePcd>          ::= "[FeaturePcd" [<com_FFArchAttrs>] "]" <EOL>
                          <FeatureEntriesStmts>*
<FeatureEntriesStmts> ::= <FeatureEntries>
<com_FFArchAttrs>     ::= {".common"} {<FFArchAttrs>}
<FFArchAttrs>         ::= <attrs> ["," <TS> "FeaturePcd" <attrs>]*
<FeatureEntries>      ::= [<NUsageBlock>]
                          <TS> <PcdName> [<FfField1>] <TailCmt>
<TailCmt>             ::= {<1UsageBlock>} {<EOL>}
<FfField1>            ::= <FS> [<Boolean>] [<FfField2>]
<FfField2>            ::= <FS> [<FFE>]
<AsBuiltPcdSec>       ::= {<BuiltPatchPcd>} {<BuiltPcdEx>}
<BuiltPatchPcd>       ::= "[PatchPcd" [<com_PPArchAttrs>] "]" <EOL>
                          <ValueOffsetPcd>*
<com_PPArchAttrs>     ::= {".common"} {<PPArchAttrs>}
<PPArchAttrs>         ::= <attrs> ["," <TS> "PatchPcd" <attrs>]*
<BuiltPcdEx>          ::= "[PcdEx" [<com_PEArchAttrs>] "]" <EOL>
                          <AbPcdEx>*
<com_PEArchAttrs>     ::= {".common"} {<PEArchAttrs>}
<attrs>               ::= "." <arch>
<PEArchAttrs>         ::= <attrs> ["," <TS> "PcdEx" <attrs>]*
<AbPcdEx>             ::= [<NUsageBlockAb>]
                          <TS> <PcdName> [<TailCmt>] <EOL>
<NUsageBlockAb>       ::= {<HiiComment>} {<NUsageBlock>}
<HiiComment>          ::= <TS> ["##" <Usage> [<MTS> <HiiInfo>] <TS>
                          <CmtOrEol>
<HiiInfo>             ::= <TS> "##" <TS> "L" <QuotedString> <FS> <CName>
                          <Offset>
<ValuePcd>            ::= <TS> <PcdName> <FS> <ValUse>
<ValUse>              ::= <AsBuiltValue> <TailCmt>
<ValueOffsetPcd>      ::= [<NUsageBlock>]
                          <TS> <PcdName> <FS> <ValOffUse>
<ValOffUse>           ::= <AsBuiltValue> <Offset> <TailCmt>
<Offset>              ::= <FS> {<LongNum>} {<UINT32>}
<AsBuiltByteArray>    ::= "{" <NList> "}"
<AsBuiltValue>        ::= if (pcddatumtype == "BOOLEAN"):
                            {"0x00"} {"0x01"} 
                          elif (pcddatumtype == "UINT8"):
                            <UINT8z> 
                          elif (pcddatumtype == "UINT16"):
                            <UINT16z>
                          elif (pcddatumtype == "UINT32"): 
                            <UINT32z>
                          elif (pcddatumtype == "UINT64"):
                            <UINT64z> 
                          else:
                            <AsBuiltByteArray>
<PcdEntries>          ::= [<NUsageBlock>]
                          <TS> <PcdName> [<PField1>] <TailCmt>
<PField1>             ::= <FS> [<Value>] [<FFE>]
<Value>               ::= if (pcddatumtype == "BOOLEAN"):
                            <Boolean>
                          elif (pcddatumtype == "UINT8"):
                            {<NumValUint8>} {<Expression>}
                          elif (pcddatumtype == "UINT16"):
                            {<NumValUint16>} {<Expression>}
                          elif (pcddatumtype == "UINT32"):
                            {<NumValUint32>} {<Expression>}
                          elif (pcddatumtype == "UINT64"):
                            {<NumValUint64>} {<Expression>}
                          else:
                            {<StringVal>} {<Expression>}
<FFE>                 ::= <FS> <FeatureFlagExpress>
<1UsageBlock>         ::= <CommentBlock>
<NUsageBlock>         ::= <CommentBlock>+
<FeatureFlagExpress>  ::= <Boolean>
<CommentBlock>        ::= <TS> ["##" <TS> <Usage>] <TS> <CmtOrEol>
<CmtOrEol>            ::= {<Comment>} {<EOL>}
<Usage>               ::= {"CONSUMES"} {"SOMETIMES_CONSUMES"}
                          {"PRODUCES"} {"SOMETIMES_PRODUCES"}
                          {"UNDEFINED"}
```

#### Parameters

**_FeatureFlagExpress_**

The feature flag expression determines whether the entry line is valid. If the
expression evaluates to `FALSE`, then the entry line is ignored by the EDK II
build system.

**_1UsageBlock_** and **_NUsageBlock_**

The 1UsageBlock location, after the entry, is preferred if there is only one
Usage for the PCD entry (this may also be referred to as a tail comment). If a
PCD has multiple usages, then all CommentBlock statements must precede the
entry.

**_Values_**

If a value is specified in an element and no value is set in the platform file,
the platform will use the value specified here, rather than the default value
specified in the DEC file that declares the PCD. The value must always match
the Datum type, as specified in the DEC file. When specifying a value for PCD
here, expression or macros are not permitted; only actual values are permitted.

**_UNDEFINED_**

Typically, this entry will be used when tools creating/installing UEFI
Distribution Packages encounter a missing or misspelled usage.

**_CONSUMES_**

This module always gets the PCD entry. This is the only usage allowed for
Feature PCDs.

**_PRODUCES_**

The module always sets the PCD entry.

**_SOMETIMES_CONSUMES_**

The module gets the PCD entry under certain conditions or execution paths.

**_SOMETIMES_PRODUCES_**

The module sets the PCD entry under certain conditions or execution paths.

**_AsBuiltByteArray_**

A byte array containing exactly the number of bytes (as specified as the
maximum number of bytes in the DSC file) used for the patchable in module PCD
when the binary was created. Any additional bytes for a value of less than the
maximum number of bytes will be zero filled. For example, if the actual value
of the array was only 4 bytes, but 10 bytes were allocated during the build,
the tools will zero fill remaining bytes (in the example, 6 additional bytes of
0x00 will be added).

#### Examples

```ini
[FixedPcd]
  gEfiMdePkgTokenSpaceGuid.PcdFSBClock|600000000
  gEfiMdePkgTokenSpaceGuid.PcdMaximumUnicodeStringLength

[FeaturePcd]
  gEfiMdePkgTokenSpaceGuid.PcdComponentNameDisable|FALSE
  gEfiMdePkgTokenSpaceGuid.PcdDriverDiagnosticsDisable

[Pcd.IA32]
  gEfiNt32PkgTokenSpaceGuid.PcdWinNtMemorySizeForSecMain

[PatchPcd.IA32]
  ## @AsBuilt
  gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel | 0x80000040 |0x00004118
```
