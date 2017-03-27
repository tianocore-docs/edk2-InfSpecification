<!--- @file
  A.2 EDK File Specification

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

## A.2 EDK File Specification

The general rules for all EDK INI style documents follow.

**********
**Note:** Path and Filename elements within the INF are case-sensitive in order
to support building on UNIX style operating systems.
**********

A section terminates with either another section definition or the end of the
file.

#### Summary

Component EDK INF description

```c
<EDK_INF> ::= [<Header>]
              <Defines>
              <Sources>
              [<Includes>]
              [<Libraries>]
              [<Nmake>]
```

### A.2.1 Header Section

#### Summary

This section contains Copyright and License notices for the INF file in
comments that start the file. This section is optional using a format of:


```ini
#/*++
#
# Copyright
# License
#
# Module Name:
# EdkFrameworkProtocolLib.inf
#
# Abstract:
#
# Component description file.
#
#--*/
```

This information a developer creating a new EDK component or library
information (INF) file.

This is an optional section.

```c
<Header>     ::= ["#"] "/*++" <EOL>
                 [<Copyright>]
                 [<License>]
                 [<ModuleName>]
                 [<Abstract>]
                 ["#"] "--*/" <EOL>
<Abstract>   ::= ["#"] "Abstract:" <EOL>
                 [["#"] <Sentence> <EOL>]*
                 ["#"] <EOL>
<ModuleName> ::= ["#"] "Module Name:" <EOL>
                 [["#"] <Sentence>+ <EOL>]+
                 ["#"] <EOL>
<Copyright>  ::= [["#"] "Copyright (c) <Date> "," <CompExtra> <EOL>]+ ["#"]
                 <EOL>
<License>    ::= [["#"] <LicenseSentence> <EOL>]+
                 ["#"] <EOL>
```

#### Example

```
#/*++
#
# Copyright (c) 2004, Intel Corporation
# All rights reserved. This program and the accompanying materials
# are licensed and made available under the terms and conditions of the
# BSD License which accompanies this distribution. The full text of the
# license may be found at
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
# Module Name:
#
# EdkFrameworkProtocolLib.inf
#
# Abstract:
#
# Component description file.
#
#--*/
```

### A.2.2 [defines] Section

#### Summary

This describes the required `[define]` tag, which is required in all EDK INF
files. This file is created by the developer and is an input to the new build
tool parsing utilities. Elements may appear in any order within this section.

This is a required section.

The define sections defines symbols that describe the component. Some items are
emitted to the output makefile.

The `FILE_GUID` is required for all EDK components that are not libraries. This
guid is used to build the FW volume file list used by build tools to generate
the final firmware volume, as well as processed in some SMM, PEI or DXE DEPEX
statements.

**********
**Note:** Possible values for `COMPONENT_TYPE`, and their descriptions, are
listed in the table, "Component (module) Types." For each component, the
`BASE_NAME` and `COMPONENT_TYPE` are required. The `COMPONENT_TYPE` definition
is case sensitive. The default FV extension can be overridden by defining the
symbol `FV_EXT`.
**********

Section `[defines.$(PROCESSOR).$(PLATFORM)]` is used with EDK components only.
The section is processed in order by the parsing utilities. Assignments of
variables in other sections do not override previous assignments.

Platform integrators that will be using EDK components that require a static
FlashMap.h (and/or FlashMap.inc) must code them by hand and maintain the state
of the static FlashMap files with the EDK II DSC and FDF files.

```c
<Defines>      ::= "[defines" [<attribs>] "]" <EOL> <expression>+
<attribs>      ::= <attrs> ["," "defines" <attrs>]*
<attrs>        ::= "." <arch> ["." <PlatformName>]
<arch>         ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {"common"}
<PlatformName> ::= {<Word>} {"$(PLATFORM)") {"platform"}
<expression>   ::= "BASE_NAME" "=" <Word> <EOL>
                   ["COMPONENT_TYPE" "=" <EdkCompType> <EOL>]
                   ["FILE_GUID" "=" <GuidOrVar> <EOL>]
                   ["EDK_RELEASE_VERSION" "=" "0x00020000" <EOL>]
                   ["MAKEFILE_NAME" "=" <Filename> <EOL>]
                   ["CUSTOM_MAKEFILE" "=" <Filename> <EOL>]
                   ["BUILD_NUMBER" "=" <Integer>{1,4} <EOL>]
                   ["BUILD_TYPE" "=" <MakefileType> <EOL>]
                   ["FFS_EXT" "=" <Word> <EOL>]
                   ["FV_EXT" "=" <Word> <EOL>]
                   ["SOURCE_FV" "=" <Word> <EOL>]
                   ["PACKAGE" "=" "CompressPEIM" <EOL>]
                   ["VERSION_NUMBER" "=" <Integer>{1,4} <EOL>]
                   ["VERSION_STRING" "=" <String> <EOL>]
                   ["GENERIC_CAPSULE_FILE_PATH" "=" <PathOnly> <EOL>]
                   ["MICROCODE_ALIGNMENT" "=" <HexNumber> <EOL>]
                   ["MICROCODE_FILE_PATH" "=" <PathOnly> <EOL>]
                   ["PLATFORM_BDS_FILE_PATH" "=" <PathOnly> <EOL>]
                   ["RESTRICTED_BDS_FILE_PATH" "=" <PathOnly> <EOL>]
<Filename>     ::= [<PATH>] <Word> ["." <Extension>]
<PathOnly>     ::= <PATH> <Word>
<MakefileType> ::= {"MAKEFILE"} {"CUSTOM_MAKEFILE"} {<Filename>}
<PATH>         ::= [<Variable> "\"] <Path>
<Path>         ::= [{<Word> "\"} {"..\"}]+
<Variable>     ::= {"$(" <MacroName> ")"}
                   {"$(EFI_SOURCE)"} {"$(EDK_SOURCE)"}
<GuidOrVar>    ::= {<RegistryFormatGUID>}
                   {"$(EFI_APRIORI_GUID)"}
                   {"$(EFI_ACPI_TABLE_STORAGE_GUID)"}
                   {"$(EFI_DEFAULT_BMP_LOGO_GUID)"}
                   {"$(EFI_PEI_APRIORI_FILE_NAME_GUID)"}
<EdkCompType>  ::= {"APPLICATION"} {"AcpiTable"} {"APRIORI"}
                   {"BINARY"} {"BS_DRIVER"} {"CONFIG"} {"FILE"}
                   {"FVIMAGEFILE"} {"LIBRARY"} {"LOGO"} {"LEGACY16"}
                   {"MICROCODE"} {"PE32_PEIM"} {"PEI_CORE"}
                   {"RAWFILE"} {"RT_DRIVER"} {"SAL_RT_DRIVER"}
                   {"SECURITY_CORE"} {"COMBINED_PEIM_DRIVER"} {"PIC_PEIM"}
                   {"RELOCATABLE_PEIM"}
<MacroName>    ::= <Word>
```

#### Example (EDK Driver)

```ini
[Defines]
  BASE_NAME      = DiskIo
  FILE_GUID      = CA261A26-7718-4b9b-8A07-5178B1AE3A02
  COMPONENT_TYPE = BS_DRIVER
```

#### Example (EDK Library)

```ini
[Defines]
  BASE_NAME      = WinNtLib
  COMPONENT_TYPE = LIBRARY
```

### A.2.3 [includes] Section

#### Summary

Defines the optional "includes paths" for EDK INF files only. These sections
should never be used in EDK II INF files. These sections are used to define the
include paths for compiling the component source files. Valid sections for EDK
include the `[includes.$(PROCESSOR).$(PLATFORM)]`, `[includes.$(PROCESSOR)]`,
and `[includes.common]` sections.

**********
**NOTE**: EDK uses both "include" and "includes" section header types. These
sections are processed if present. These paths are used to define the `$(INC)`
macro and is written to the component's makefile.
**********

This is an optional section.

The standard Macro Definitions are not permitted within this section.

For EDK modules, the path must include either the `$(EFI_SOURCE)` or
`$(EDK_SOURCE)` environment variable.

This section also allows for specifying individual header files that will be
added to the `$(INC)` macro using the `/FI` (Microsoft) or `-include` (GCC)
switch. This is an optional section.

```c
<Includes>   ::= "[include" ["s"] [<Attrs>] ']" <EOL>
                 <PATH>+
<Attrs>      ::= <Attributes> ["," "include" ["s"]? <Attrs>]*
<Attributes> ::= [<Archs>] [<VarName>] [<Platform>]
<Archs>      ::= "." <arch>
<arch>       ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {"common"} {<OA>}
<VarName>    ::= "." {"$(PROCESSOR)"} {"$(" <Word> ")"}
<Platform>   ::= "." {"Platform") {"nt32"} {"$(PLATFORM)"}
<PATH>       ::= {<MACRO>} {<RelDir>}
<MACRO>      ::= <MacroName> ["\" <Word>]+
<MacroName>  ::= {"$(EDK_SOURCE)"} {"$(EFI_SOURCE)"} {"$(BUILD_DIR)"}
                 {"$(SOURCE_DIR)"}
<RelDir>     ::= ["..\"]+ {".."} {<Word>}
```

#### Example

```ini
[Includes.common]
  $(EDK_SOURCE)FoundationEfi
  $(EDK_SOURCE)Foundation
  $(EDK_SOURCE)FoundationFramework
  .
  $(EDK_SOURCE)FoundationInclude
  $(EDK_SOURCE)FoundationEfiInclude
  $(EDK_SOURCE)FoundationFrameworkInclude
  $(EDK_SOURCE)FoundationIncludeIndustryStandard
  $(EDK_SOURCE)FoundationCoreDxe
  $(EDK_SOURCE)FoundationLibraryDxeInclude
```

### A.2.4 [libraries] Section

#### Summary

Defines the optional `[libraries]` section tag for EDK INF files. The
`[libraries]` section is used to define a `$(LIBS)` macro in the EDK component's
makefile. All libraries listed in the `[libraries.common]`,
`[libraries.$(PROCESSOR)]`, and `[libraries.$(PROCESSOR).$(PLATFORM)]` sections
are added to the LIBS definition as either `$(LIB_DIR)\LibName` or
`$(PROCESSOR)\LibName`. The libraries are specified without path information.

The standard Macro Definitions are not permitted in this section.

This is an optional section.

```c
<Libraries> ::= "[libraries" [<attrs>] ']" <EOL> <LibName>+
<attrs>     ::= "." <archs> ["." <platform>]
<archs>     ::= <arch> ["," <arch> ["." <platform>]]
                ["," "libraries." <attrs>]*
<arch>      ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {"common"}
                {"platform"} {"nt32"} {"$(PROCESSOR)"}
<platform>  ::= {"platform"} {"$(PLATFORM)"} {<Word>}
<LibName>   ::= <Word>
```

#### Example

```ini
[Libraries.common]
  EfiProtocolLib
  EfiDriverLib
```

### A.2.5 [nmake] Section

#### Summary

Defines the [nmake] section tag for EDK INF files. These sections are used to
make a direct addition to the component's output makefile. EFI section
`[nmake.$(PROCESSOR).$(PLATFORM)]`, `[nmake.$(PROCESSOR)]`, and `[nmake.common]`
are processed if present. The section data is simply copied directly to the
component makefile, before the build commands are emitted. Filenames specified
in this section are relative to the EDK INF file or the EDK DSC file.

This is an optional section.

This section is not permitted in EDK II modules. Note that the `C_STD_INCLUDE`
line is usually used to clear any flags that might have been set by the
Microsoft tool chain setup scripts, therefore no `<CFlags>` are printed, and
the line in the Example is the most common usage.

The standard Macro Definitions are not permitted in this section.

```c
<Nmake>      ::= "[nmake" [<attrs>] ']" <EOL>
                 <expression>+
<attrs>      ::= <Archs> [<plat>] ["," "nmake" <attrs>]*
<Archs>      ::= "." <arch>
<plat>       ::= "." <Word>
<arch>       ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {"common"}
<expression> ::= [<IEP>] [<DS>] [<CSI>] [<CPF>] [<APF>] [<LSF>] [<EBC>] [<DEF>]
<IEP>        ::= "IMAGE_ENTRY_POINT" "=" <CName> <EOL>
<DS>         ::= "DPX_SOURCE" "=" <Filename> <EOL>
<CSI>        ::= "C_STD_INCLUDE" "=" [<CFlags>] <EOL>
<CPF>        ::= "C_PROJ_FLAGS" "=" <CFlags> <EOL>
<APF>        ::= "ASM_PROJ_FLAGS" "=" <AFlags> <EOL>
<LSF>        ::= "LIB_STD_FLAGS" "=" <LFlags> <EOL>
<EBC>        ::= ["EBC_C_STD_FLAGS" "=" <String> <EOL>]
                 ["EBC_LIB_STD_FLAGS" "=" <String> <EOL>]
<DEF>        ::= <Word> "=" {<Word>} {<String>} <EOL>
<STATEMENTS> ::= Valid NMAKE Makefile syntax.
```

#### Parameters

**_CFlags_**

This content must be valid compiler flags for the Microsoft C compiler, cl.exe.

**_AFlags_**

This content must be valid flags for the Microsoft Assembler, ml.exe.

**_LFlags_**

This content must be valid flags for the Microsoft Linker, lib.exe.

#### Example

```ini
[nmake.common]
  C_STD_INCLUDE =
  IMAGE_ENTRY_POINT=WatchdogTimerDriverInitialize
  DPX_SOURCE=WatchDogTimer.dxs

[nmake.common]
  C_FLAGS       = $(C_FLAGS) /D EDKII_GLUE_LIBRARY_IMPLEMENTATION
  LIB_STD_FLAGS = $(LIB_STD_FLAGS) /IGNORE:4006 /IGNORE:4221

[nmake.ia32]
  C_FLAGS = $(C_FLAGS) /D MDE_CPU_IA32

[nmake.x64]
  C_FLAGS = $(C_FLAGS) /D MDE_CPU_X64

[nmake.ipf]
  C_FLAGS = $(C_FLAGS) /D MDE_CPU_IPF

[nmake.ebc]
  EBC_C_STD_FLAGS   = $(EBC_C_STD_FLAGS) /D EDKII_GLUE_LIBRARY_IMPLEMENTATION
  EBC_LIB_STD_FLAGS = $(EBC_LIB_STD_FLAGS) /IGNORE:4006 /IGNORE:4221
  EBC_C_STD_FLAGS   = $(EBC_C_STD_FLAGS) /D MDE_CPU_EBC
```

### A.2.6 [sources] Section

#### Summary

Defines the `[sources]` section tag is required for EDK INF files. NOTE: EDK
uses both "source" and "sources" in the section header.

There can be multiple sources sections, depending on the target processor.
Example sources sections are listed below. The parsing utility creates a
directory path for each file ($(DEST_DIR)...\MyFile.c), and looks up the
makefile template for the COMPONENT_TYPE (EDK) to emit.

It is not permissible to mix EDK and EDK II style files within a module.

The macro, TABLE_NAME may be used in existing EDK INF files that point to ACPI
tables, this value wil be ignored by EDK II build tools.

```c
<sources>         ::= "[source" ["s"] [<attrs>] "]" <EOL>
                      [<MacroDefinition>]* [<EdkExpression>]+
<attrs>           ::= <Archs> ["," "sources" <attrs>]*
<Archs>           ::= if (COMPONENT_TYPE defined in defines):
                      "." <archs> [<plat>] else:
                      "." <archs>
<archs>           ::= if (COMPONENT_TYPE defined in defines): <EdkArch> [","
                      <EdkArch>]* else:
                      <arch> ["|" <arch>]*
<EdkArch>         ::= {"IA32"} {"X64"} {"IPF"} {"Common"}
<plat>            ::= "." {"$(PLATFORM)"} {"$(PROCESSOR)"} {<Word>}
<DefineStatement> ::= ["DEFINE" <Word> "=" [<PATH>] <EOL>]*
                      ["TABLE_NAME" "=" <Word> <EOL>]
<EdkExpression>   ::= <Filename> ["|" <Family>] <EOL>
<Family>          ::= {"MSFT"} {"GCC"} {"INTEL"} {"*"}
<Filename>        ::= <Path> <Word> "." <Extension>
<Path>            ::= [<Macro> {"\"} {"/"}] [<Word> {"\"} {"/"}]+
```

#### Examples

```ini
[sources.common]
  BsDataHubStatusCode.c
  BsDataHubStatusCode.h
```
