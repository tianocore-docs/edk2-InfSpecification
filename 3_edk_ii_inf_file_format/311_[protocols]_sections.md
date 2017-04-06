<!--- @file
  3.11 [Protocols] Sections

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

## 3.11 [Protocols] Sections

These are optional sections. If the source module code contains any protocols,
then this section must be generated in the "As Built" INF file listing each
protocol with its `<CommentBlock>` content.

#### Summary

Defines the optional EDK II INF file `[Protocols]` section tag. This is a list
of the global PROTOCOL C Names that are referenced in the EDK II Module's C
code.

Each protocol must be listed only once per section. Protocols listed in
architectural sections are not permitted to be listed in the common
architectural section.

The "common" architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

The format of the `<CommentBlock>` is the recommended format that will
guarantee that the information is correctly inserted into UEFI Distribution
Package description files by the Intel(R) UEFI Packaging Tool included in the
EDK II base tools project. The usages in the comment block describe how the
Protocol is used in the C code.

A binary INF file must not contain any `FeatureFlagExpression` content.

#### Prototype

```c
<Protocols>          ::= "[Protocols" [<com_attribs>] "]" <EOL>
                         <ProtoStatments>*
<com_attribs>        ::= {".common"} {<attribs>}
<attribs>            ::= <attrs> ["," <TS> "Protocols" <attrs>]*
<attrs>              ::= "." <arch>
<ProtoStatements>    ::= [<NUsageBlock>]
                         <TS> <ProtocolSpec> {<1UsageBlock>} {<EOL>}
<ProtocolSpec>       ::= <CName> [<FS> <FeatureFlagExpress>]
                         <1
UsageBlock>          ::= <CommentBlock>
<NUsageBlock>        ::= <CommentBlock>+
<FeatureFlagExpress> ::= <Boolean>
<CommentBlock>       ::= <TS> <UsageField> <TS> [<Comment>] <EOL>
<UsageField>         ::= ["##" <TS> <Usage> <TS>] [<Notify>]
<Notify>             ::= "##" <TS> "NOTIFY" <TS>
<Usage>              ::= {"PRODUCES"} {"SOMETIMES_PRODUCES"}
                         {"CONSUMES"} {"SOMETIMES_CONSUMES"}
                         {"TO_START"} {"BY_START"} {"UNDEFINED"}
```

#### Parameters

**_FeatureFlagExpress_**

When present, the feature flag expression determines whether the entry line is
valid. If the feature flag expression evaluates to FALSE, this entry will be
ignored by the EDK II build tools.

**_1UsageBlock_** and **_NUsageBlock_**

The `1UsageBlock` location, after the entry, is preferred if there is only one
usage for the Protocol entry. If a Protocol has multiple usages, then all
`CommentBlock` statements must precede the entry.

**_UNDEFINED_**

Typically, this entry will be used when tools creating/installing UEFI
Distribution Packages encounter a missing or misspelled usage. `UNDEFINED` is
also valid when the Protocol is not used as a Protocol and the GUID value of
the Protocol is used for something else.

**_CONSUMES_**

This module does not install the protocol, but needs to locate a protocol. Not
valid if the `Notify` attribute is true.

**_PRODUCES_**

This module will install this protocol. Not valid if the `Notify` attribute is
true.

**_SOMETIMES_CONSUMES_**

This module does not install the protocol, but may need to locate a protocol
under certain conditions, (such as if it is present.) If the `Notify` attribute
is set, then the module will use the protocol, named by GUID, via a registry
protocol notify mechanism.

**_SOMETIMES_PRODUCES_**

This module will install this protocol under certain conditions. Not valid if
the `Notify` attribute is true.

**_TO_START_**

The protocol is consumed by a Driver Binding protocol Start function. Thus the
protocol is used as part of the UEFI driver model. Not valid if the `Notify`
attribute is true.

**_BY_START_**

The protocol is produced by a Driver Binding protocol Start function. Thus the
protocol is used as part of the UEFI driver model. Not valid if the `Notify`
attribute is true.

**_NOTIFY_**

This specifies whether this is a Protocol or ProtocolNotify. If set, then the
module will use this protocol, named by GUID, via a registry protocol notify
mechanism.

#### Example

```ini
[protocols]
  gEfiDecompressProtocolGuid
  gEfiLoadFileProtocolGuid
```
