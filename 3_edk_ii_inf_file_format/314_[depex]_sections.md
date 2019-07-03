<!--- @file
  3.14 [Depex] Sections

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

## 3.14 [Depex] Sections

These are optional sections

#### Summary

Defines the optional EDK II INF file `[Depex]` section content. The `[Depex]`
section is a replacement for the dependency file specified by the driver
writer. This section can be used for inheritance from libraries, by supporting 
logical AND'ing of the different Depex expressions together.

The Rules would be as follows:

* EDK II INF - `[Depex]` section and inheritance from libraries is supported
  via AND'ing the different Depex expressions together

* EDK II INF - The `[Defines]` section's keyword, `DPX_SOURCE`, would override
  Depex section and let module owner force a Depex independent of the `[Depex]`
  inheritance. Not recommended, but gives complete control to the driver writer.

* Each `[Depex]` section tag listed in an INF file must be unique. If there are
  multiple `[Depex]` sections that have the same section tag, i.e.,
  `[Depex.IA32.DXE_DRIVER]` and another `[Depex.IA32.DXE_DRIVER]` section in the
  same INF, the build must break.

If a `DPX_SOURCE` is specified in the `[Defines]` section, the `[Depex]`
section is ignored, and the file specified in the `DPX_SOURCE` is used instead.

When processing the file, the INF file name specified in the `<GuidStmt>` and
`<DepInstruct>` statement is replaced by the `FILE_GUID` value from the INF
file, translated to a POSIX C structure as shown below:

```c
INTERFACENAME = { /* 0F05DE03-8A1B-408C-8F84-B547F593E463 */
    0x0F05DE03,
    0x8A1B,
    0x408C,
    {0x8F, 0x84, 0xB5, 0x47, 0xF5, 0x93, 0xE4, 0x63}
  };
```

The term, "SOR" is ignored as part of the dependency processing. The DXE
driver is to remain on the Schedule on Request (`SOR`) queue until the DXE
Service `Schedule()` is called for this DXE. The dependency expression
evaluator treats this operation like a No Operation (`NOP`).

There are three types of dependency sections (PEI, SMM and DXE) permitted by
specifications. The SMM dependency section uses the same grammar as the DXE
dependency section. The optional tags (identified as `<DepexType>` in the EBNF,
below) must be used at the start of a depex listing. The depex expression for a
given type is terminated by the start of a new optional section tag, the start
of a new section or the end of file.

Drivers with `MODULE_TYPE` set to `SEC`, `PEI_CORE`, `DXE_CORE`, `SMM_CORE`,
`UEFI_DRIVER` and `UEFI_APPLICATION` cannot have `[Depex]` sections. Libraries
and modules that are `USER_DEFINED` may have a `[Depex]` section. All remaining
drivers, `PEIM`, `DXE_DRIVER`, `DXE_SAL_DRIVER`, `DXE_RUNTIME_DRIVER` and
`DXE_SMM_DRIVER` module types must have a `[Depex]` section.

Libraries of type `SEC`, `PEI_CORE`, `DXE_CORE`, `SMM_CORE and`
`UEFI_APPLICATION` are not allowed to have a `[Depex]`. The `MODULE_TYPE` entry
in the `[Defines]` section for a library only defines the module type that the
build system must assume for building the library. It does not define the types
of modules that a library may be linked with. Instead, the `LIBRARY_CLASS`
entry in the `[Defines]` section optionally lists the supported module types
that the library may be linked against.

Libraries of type `BASE` are not permitted to have generic (i.e., `[Depex]`) and
generic with only architectural modifier (i.e., `[Depex.IA32]`) entries. Library
of type `BASE` are permitted to have a Depex section if one ModuleType modifier
is specified (i.e., `[Depex.common.PEIM`).

When using the ModuleType as a section modifier (for example:
[Depex.IA32.PEIM]), for drivers, the ModuleType must match the value of
`MODULE_TYPE` entry in the `[Defines]` section. For Libraries, the ModuleType
used in the section modifier must be a member of the Module Types listed after
the `LIBRARY_CLASS` keyword in the `[Defines]` section. If no module types are
listed after the `LIBRARY_CLASS` keyword in the `[Defines]` section, then the
library is compatible with all module types, so all module types may be used as
a section modifier.

The "common" architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

If the `MODULE_TYPE` is `UserDefined`, the build tools must exit gracefully and
provide the user with an error message stating that the `[Depex]` section
header does not provide enough information to determine the type of the Depex
section.

If the module is not a library (no `LIBRARY_CLASS` in the `[Defines]` section)
and the `MODULE_TYPE` is `SEC`, `SMM_CORE`, `DXE_CORE`, `PEI_CORE`,
`UEFI_DRIVER`, `UEFI_APPLICATION` or `HOST_APPLICATION` a Depex section is
not permitted. If one is found, the build tools must exit gracefully and
provide the user with an error message stating the `[Depex]` section is not
valid for the `MODULE_TYPE`

If the module is a library (with a `LIBRARY_CLASS` statement in the `[Defines]`
section) and there is no module type defined in Depex section's modifier and
there is a MTL defined (with a ModuleTypeList statement following by the
`LIBRARY_CLASS` statement in the `[Defines]` section) and each module type in
MTL is `PEIM,`

`DXE_DRIVER`, `DXE_SAL_DRIVER`, `DXE_RUNTIME_DRIVER`, `DXE_SMM_DRIVER` or
`UEFI_DRIVER` the build tools must create a related Depex section for each
module type in XML.

If the module is a library and the `MODULE_TYPE` is not `BASE` (with a
`LIBRARY_CLASS` statement in the `[Defines]` section) and it has no MTL defined
(without a `ModuleTypeList` statement following by the `LIBRARY_CLASS`
statement in the `[Defines]` section) a Depex section is not permitted. If one
is found, the build tools must exit gracefully and provide the user with an
error message stating the `[Depex]` section is not valid for the module.

If the module is a library (with a `LIBRARY_CLASS` statement in the [Defines]
section) and the `MODULE_TYPE` is `UEFI_DRIVER`, the `[Depex]` section must map
to a `DxeDepex` section in the XML.

For a binary INF file, the `[Depex]` section will contain the full dependency
expression, including the dependencies from the linked libraries in a comment.

**********
**Note:** A UEFIDRIVER which is not included in an FD image (such as a driver
that will be loaded from the shell or stored in a PCI option ROM) will not have
an FFS DEPEX section generated by the tools.
**********
**Note:** Capitalization in the Prototypes listed below does not match the
capitalization used in the PI Specification.
**********
**Note:** The PCD item used in this section must be defined as FixedAtBuild type
and VOID* datum type, and the size of the PCD must be 16 bytes.
**********

#### Prototype

```c
<Depex>              ::= "[Depex" [<com_attribs>] "]" <EOL>
                         {<DepexSection>*} {<AsBuiltDepex>}
<com_attribs>        ::= {".common"} {<attribs>}
<attribs>            ::= <attrs> ["," <TS> "Depex" <attrs>]*
<attrs>              ::= "." <arch> [<Module>]
<Module>             ::= "." <DepexModuleType>
<DepexModuleType>    ::= {"PEIM"} {"DXE_DRIVER"} {"DXE_SMM_DRIVER"}
                         {"DXE_RUNTIME_DRIVER"} {"DXE_SAL_DRIVER"}
                         {"UEFI_DRIVER"} {"USER_DEFINED"}
<DepexSection>       ::= {<PeiDepex>} {<SmmDepex>} {<DxeDepex>}
<DxeDepex>           ::= <DxeDepexStatements>+ ["END" <EOL>]
<DxeDepexStatements> ::= {<SorStmt>} {<GuidOrPcdStmt>} {<BoolStmt>}
<PeiDepex>           ::= <PeiDepexStatements>*
                         ["END" <EOL>]
<PeiDepexStatements> ::= {<BoolStmt>} {<DepInstruct>}
<SmmDepex>           ::= <DxeDepex>
<GuidOrPcdStmt>      ::= [{"BEFORE"} {"AFTER"}] <GuidOrPcdName> [<EOL>]
<GuidOrPcdName>      ::= {<GuidCName>} {<PcdName>}
<DepInstruct>        ::= "PUSH" <CFormatGUID> [<EOL>]
<SorStmt>            ::= "SOR" <BoolStmt> [<EOL>]
<BoolStmt>           ::= {<Bool>} {<BoolExpress>}
<Bool>               ::= {"TRUE"} {"FALSE"} {<GuidCName>} {<PcdName>} [<EOL>]
<GuidCName>          ::= <CName> # A Guid C Name
<NTs>                ::= "NOT" <TS>
<BoolExpress>        ::= <NTs> <Bool> [<BoolExpressCont>]*
<BoolExpressCont>    ::= <MTS> {"AND"} {"OR"} <MTS> [<NTs>] <Bool>
<AsBuiltDepex>       ::= "#" {<FullDxe>} {<FullPei>} {<FullSmm>} <EOL>
<FullDxe>            ::= <DxeStatements>+
                         [<TS> "END"]
<DxeStatements>      ::= {<SorAbStmt>} {<GuidOrPcdAbStmt>} {<BoolAbStmt>}
<FullPei>            ::= <PeiStatements>*
                         [<TS> "END"]
<PeiStatements>      ::= {<BoolAbStmt>} {<DepAbInstruct>}
<FullSmm>            ::= <FullDxe>
<GuidOrPcdAbStmt>    ::= {"BEFORE"} {"AFTER"} <TS> <GuidOrPcdName> <TS>
<DepAbInstruct>      ::= "PUSH" <TS> <CFormatGUID> <TS>
<SorAbStmt>          ::= "SOR" <TS> <BoolAbStmt> <TS>
<BoolAbStmt>         ::= {<BoolAb>} {<BoolAbExpress>}
<BoolAb>             ::= {"TRUE"} {"FALSE"} {<GuidCName>} {<PcdName>} <TS>
<GuidCName>          ::= <CName> # A Guid C Name
<BoolAbExpress>      ::= <NTs> <Bool> [<BoolAbExpressCont>]*
<BoolAbExpressCont>  ::= <MTS> {"AND"} {"OR"} <MTS> [<NTs>] <Bool>
```

#### Example

```ini
[Depex]
  gEfiFirmwareVolumeBlockProtocolGuid
  AND gEfiAlternateFvBlockGuid
  AND gEfiFaultTolerantWriteLiteProtocolGuid

[Depex]
  gEfiNt32PkgTokenSpaceGuid.PcdGuidTest

[Depex.common]
  SOR gEfiProtocolIDependOnGuid

[Depex.IA32.DXE_SMM_DRIVER]
  TRUE

[Depex.IA32.DXE_DRIVER]
  TRUE AND gEfiAlternateFvBlockGuid
```
